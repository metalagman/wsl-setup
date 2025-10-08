# Repository Guidelines

## Project Structure & Module Organization
WSL automation lives in `playbooks/site.yml`, which stitches together the role sequence (`common`, `golang`, `devops`, `nodejs`, `ai`) and conditionally applies the desktop role (`ubuntu-desktop`) when `env == "ubuntu-desktop"`. Each role follows the standard Ansible layout under `roles/<role>/tasks/main.yml` with supporting `files` or `templates` folders when needed. Host configuration lives in `inventory/wsl2.yml` (default via `ansible.cfg`) or `inventory/ubuntu-desktop.yml`. Bootstrap helpers (`preprovision.sh`) and the task runner (`Taskfile.yml`) are in the repository root.

## Build, Test, and Development Commands
- `sudo ./preprovision.sh`: install Ansible and prerequisites on a fresh system.
- `task provision`: run the full provisioning flow via Taskfile (defaults to the WSL2 inventory). Override with `task provision INVENTORY=inventory/ubuntu-desktop.yml` to target Ubuntu Desktop.
- Direct playbook runs:
  - WSL2: `sudo ansible-playbook -i inventory/wsl2.yml playbooks/site.yml`
  - Ubuntu Desktop: `sudo ansible-playbook -i inventory/ubuntu-desktop.yml playbooks/site.yml`
- `ansible-playbook --syntax-check playbooks/site.yml`: verify playbook syntax without touching the host.

## Coding Style & Naming Conventions
Use two-space indentation in YAML and keep keys alphabetized where it improves readability. Name task files with concise, lower_snake_case identifiers (`roles/devops/tasks/main.yml`). Role directories are lowercase and may be hyphenated when appropriate (e.g., `roles/ubuntu-desktop`) to align with inventory/env naming. Prefer idempotent Ansible modules over `shell`/`command`, and guard commands with `creates` or `when` checks. When adding variables, document defaults either in `roles/<role>/defaults/main.yml` or within `group_vars` if expanded later.

## Testing Guidelines
Run a check/diff before submitting changes to preview effects:
- WSL2: `ansible-playbook -i inventory/wsl2.yml playbooks/site.yml --check --diff`
- Ubuntu Desktop: `ansible-playbook -i inventory/ubuntu-desktop.yml playbooks/site.yml --check --diff`
Target individual roles with `--tags` or `--start-at-task` when troubleshooting. If `ansible-lint` is available in your environment, include it in your local checks to catch style and best-practice issues early.

## Commit & Pull Request Guidelines
Commit messages in this repo use short, imperative summaries (e.g., `add codex playbook`, `update readme`). Follow that tone and keep the first line under 72 characters. In pull requests, describe the motivation, list the roles or hosts touched, and mention any follow-up actions required after merge. Attach command output or logs when provisioning was validated locally.

## Security & Configuration Tips
Keep secrets out of version control; prefer environment variables or encrypted vault files when expanding beyond localhost. Double-check `inventory/wsl2.yml` (the default inventory in `ansible.cfg`) before committing to ensure only safe defaults ship. When introducing third-party installers or APIs, note required tokens in the README and guard networked tasks with `when` clauses so offline runs remain stable.
