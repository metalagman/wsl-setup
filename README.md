# WSL (Windows Subsystem for Linux) Setup with Ansible

[![Ansible Lint](https://github.com/metalagman/wsl-setup/actions/workflows/ansible-lint.yml/badge.svg)](https://github.com/metalagman/wsl-setup/actions/workflows/ansible-lint.yml)

This repository contains an Ansible playbook to automate the setup of a development and DevOps environment on a fresh WSL (Windows Subsystem for Linux) installation.

## Requirements

*   A WSL (Windows Subsystem for Linux) distribution (e.g., Ubuntu).
*   `sudo` privileges on the WSL instance.

## Installation

1.  **Install Ansible and prerequisites:**

    ```bash
    sudo apt update
    sudo apt install -y software-properties-common git
    sudo add-apt-repository --yes --update ppa:ansible/ansible
    sudo apt install -y ansible
    ```

2.  **Clone the repository:**

    ```bash
    git clone https://github.com/metalagman/wsl-setup.git
    cd wsl-setup
    ```

3.  **Run Ansible playbook:**

    Choose one of the targets below and run it. The inventory is preconfigured in `ansible.cfg`.

    - WSL2 environment:

      ```bash
      sudo ansible-playbook playbooks/wsl2.yml
      ```

    - Ubuntu Desktop (bare-metal or VM):

      ```bash
      sudo ansible-playbook playbooks/ubuntu-desktop.yml
      ```

## Usage

This repo now uses a single common inventory for localhost and separate playbooks for each target environment.

- WSL2 environment:

```bash
sudo ansible-playbook playbooks/wsl2.yml
```

- Ubuntu Desktop (bare-metal or VM):

```bash
sudo ansible-playbook playbooks/ubuntu-desktop.yml
```

Note: ansible.cfg sets the default inventory to `inventory/localhost.yml`, so no `-i` flag is required.

Enter your `sudo` password when prompted.

### Using the Taskfile

This project uses [Task](https://taskfile.dev/) as a command runner.

After installing the dependencies with the `preprovision.sh` script, you can use the following commands:

*   Provision with Ansible using the common inventory:

    - WSL2

      ```bash
      task provision:wsl2
      ```

    - Ubuntu Desktop

      ```bash
      task provision:ubuntu-desktop
      ```

### Running Claude with a Local Model

To use Claude with a local model server, configure the following environment variables:

```bash
export ANTHROPIC_AUTH_TOKEN="local-key"
export ANTHROPIC_BASE_URL=http://localhost:3456
claude
```