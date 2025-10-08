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

2.  **Run the pre-provisioning script:**

    This script will install Ansible and other dependencies.

    ```bash
    sudo chmod +x preprovision.sh
    sudo ./preprovision.sh
    ```

## Usage

Choose the appropriate inventory for your target environment and run the playbook:

- WSL2 environment:

```bash
sudo ansible-playbook -i inventory/wsl2.yml playbooks/site.yml
```

- Ubuntu Desktop (bare-metal or VM):

```bash
sudo ansible-playbook -i inventory/ubuntu-desktop.yml playbooks/site.yml
```

Note: ansible.cfg may point to a default inventory, but using -i explicitly selects the desired inventory per run.

Enter your `sudo` password when prompted.

### Using the Taskfile

This project uses [Task](https://taskfile.dev/) as a command runner.

After installing the dependencies with the `preprovision.sh` script, you can use the following commands:

*   Provision with Ansible using a specific inventory:

    - WSL2

      ```bash
      task provision INVENTORY=inventory/wsl2.yml
      ```

    - Ubuntu Desktop

      ```bash
      task provision INVENTORY=inventory/ubuntu-desktop.yml
      ```

### Running Claude with a Local Model

To use Claude with a local model server, configure the following environment variables:

```bash
export ANTHROPIC_AUTH_TOKEN="local-key"
export ANTHROPIC_BASE_URL=http://localhost:3456
claude
```