# Repository Guidelines

## Project Structure & Module Organization
Automation now uses a common localhost inventory at `inventory/localhost.yml` and separate playbooks for environments:
- `playbooks/wsl2.yml` applies `common`, `golang`, `devops`, `nodejs`, `ai`.
- `playbooks/ubuntu-desktop.yml` applies `ubuntu-desktop` first, then the same common stack.
Each role follows the standard Ansible layout under `roles/<role>/tasks/main.yml` with supporting `files` or `templates` folders when needed. Bootstrap helpers (`preprovision.sh`) and the task runner (`Taskfile.yml`) are in the repository root.

## Build, Test, and Development Commands
- `sudo ./preprovision.sh`: install Ansible and prerequisites on a fresh system.
- Task runner:
  - `task provision:wsl2` — run WSL2 provisioning
  - `task provision:ubuntu-desktop` — run Ubuntu Desktop provisioning
- Direct playbook runs (common inventory):
  - WSL2: `sudo ansible-playbook -i inventory/localhost.yml playbooks/wsl2.yml`
  - Ubuntu Desktop: `sudo ansible-playbook -i inventory/localhost.yml playbooks/ubuntu-desktop.yml`
- Syntax checks:
  - `ansible-playbook --syntax-check playbooks/wsl2.yml`
  - `ansible-playbook --syntax-check playbooks/ubuntu-desktop.yml`

## Coding Style & Naming Conventions
Use two-space indentation in YAML and keep keys alphabetized where it improves readability. Name task files with concise, lower_snake_case identifiers (`roles/devops/tasks/main.yml`). Role directories are lowercase and may be hyphenated when appropriate (e.g., `roles/ubuntu-desktop`) to align with inventory/env naming. Prefer idempotent Ansible modules over `shell`/`command`, and guard commands with `creates` or `when` checks. When adding variables, document defaults either in `roles/<role>/defaults/main.yml` or within `group_vars` if expanded later.

## Testing Guidelines
Run a check/diff before submitting changes to preview effects:
- WSL2: `ansible-playbook -i inventory/localhost.yml playbooks/wsl2.yml --check --diff`
- Ubuntu Desktop: `ansible-playbook -i inventory/localhost.yml playbooks/ubuntu-desktop.yml --check --diff`
Target individual roles with `--tags` or `--start-at-task` when troubleshooting. If `ansible-lint` is available in your environment, include it in your local checks to catch style and best-practice issues early.

## Commit & Pull Request Guidelines
Commit messages in this repo use short, imperative summaries (e.g., `add codex playbook`, `update readme`). Follow that tone and keep the first line under 72 characters. In pull requests, describe the motivation, list the roles or hosts touched, and mention any follow-up actions required after merge. Attach command output or logs when provisioning was validated locally.

## Security & Configuration Tips
Keep secrets out of version control; prefer environment variables or encrypted vault files when expanding beyond localhost. Double-check `inventory/localhost.yml` (the default inventory in `ansible.cfg`) before committing to ensure only safe defaults ship. When introducing third-party installers or APIs, note required tokens in the README and guard networked tasks with `when` clauses so offline runs remain stable.
