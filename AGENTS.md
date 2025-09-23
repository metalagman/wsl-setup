# Repository Guidelines

## Project Structure & Module Organization
WSL automation lives in `playbooks/site.yml`, which stitches together the role sequence (`common`, `golang`, `devops`, `nodejs`, `ai`). Each role follows the standard Ansible layout under `roles/<role>/tasks/main.yml` with supporting `files` or `templates` folders when needed. Host configuration lives in `inventory/localhost.yml`, and repo-wide defaults sit in `ansible.cfg`. Bootstrap helpers (`preprovision.sh`) and the task runner (`Taskfile.yml`) are in the repository root.

## Build, Test, and Development Commands
- `sudo ./preprovision.sh`: install Ansible and prerequisites on a fresh WSL instance.
- `task provision`: run the full provisioning flow via Taskfile (alias for the site playbook).
- `sudo ansible-playbook -i inventory/localhost.yml playbooks/site.yml`: execute the site playbook directly.
- `ansible-playbook --syntax-check playbooks/site.yml`: verify playbook syntax without touching the host.

## Coding Style & Naming Conventions
Use two-space indentation in YAML and keep keys alphabetized where it improves readability. Name roles and task files with concise, lower_snake_case identifiers (`roles/devops/tasks/main.yml`). Prefer idempotent Ansible modules over `shell`/`command`, and guard commands with `creates` or `when` checks. When adding variables, document defaults either in `roles/<role>/defaults/main.yml` or within `group_vars` if expanded later.

## Testing Guidelines
Run `ansible-playbook -i inventory/localhost.yml playbooks/site.yml --check --diff` before submitting changes to preview effects. Target individual roles with `--tags` or `--start-at-task` when troubleshooting. If `ansible-lint` is available in your environment, include it in your local checks to catch style and best-practice issues early.

## Commit & Pull Request Guidelines
Commit messages in this repo use short, imperative summaries (e.g., `add codex playbook`, `update readme`). Follow that tone and keep the first line under 72 characters. In pull requests, describe the motivation, list the roles or hosts touched, and mention any follow-up actions required after merge. Attach command output or logs when provisioning was validated locally.

## Security & Configuration Tips
Keep secrets out of version control; prefer environment variables or encrypted vault files when expanding beyond localhost. Double-check `inventory/localhost.yml` before committing to ensure only safe defaults ship. When introducing third-party installers or APIs, note required tokens in the README and guard networked tasks with `when` clauses so offline runs remain stable.
