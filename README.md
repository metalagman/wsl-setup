# WSL (Windows Subsystem for Linux) Setup with Ansible

[![Ansible Lint](https://github.com/metalagman/wsl-setup/actions/workflows/ansible-lint.yml/badge.svg)](https://github.com/metalagman/wsl-setup/actions/workflows/ansible-lint.yml)

This repository contains an Ansible playbook to automate the setup of a development and DevOps environment on a fresh WSL (Windows Subsystem for Linux) installation.

## Requirements

*   A WSL (Windows Subsystem for Linux) distribution (e.g., Ubuntu).
*   `sudo` privileges on the WSL instance.

## Installation

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/metalagman/wsl-setup.git
    cd wsl-setup
    ```

2.  **Install Ansible and prerequisites:**

    ```bash
    chmod +x preprovision.sh
    ./preprovision.sh
    ```

3.  **Run Ansible playbook:**

    Choose one of the targets below and run it. The inventory is preconfigured in `ansible.cfg` to use `inventory/localhost.yml`.

    - WSL2 environment:

      ```bash
      sudo ansible-playbook playbooks/wsl2.yml
      ```

    - Ubuntu Desktop (bare-metal or VM):

      ```bash
      sudo ansible-playbook playbooks/ubuntu-desktop.yml
      ```

    - Ubuntu Server:

      ```bash
      sudo ansible-playbook playbooks/ubuntu-server.yml
      ```

    Enter your `sudo` password when prompted.

## Usage

After the initial run, you can use [Task](https://taskfile.dev/) (which is installed during the first provision) to run updates:

### Using the Taskfile

- Provision with Ansible using the common inventory:

    - WSL2

      ```bash
      task provision:wsl2
      ```

    - Ubuntu Desktop

      ```bash
      task provision:ubuntu-desktop
      ```

    - Ubuntu Server

      ```bash
      task provision:ubuntu-server
      ```

- Run linting on the playbooks and roles:

    ```bash
    task lint
    ```

### Running Claude with a Local Model

To use Claude with a local model server, configure the following environment variables:

```bash
export ANTHROPIC_AUTH_TOKEN="local-key"
export ANTHROPIC_BASE_URL=http://localhost:3456
claude
```