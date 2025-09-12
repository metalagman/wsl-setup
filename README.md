# WSL Setup with Ansible

Ansible playbook to set up development and DevOps tools on fresh WSL.

## Setup

1. **Install Ansible:**
   ```bash
   sudo chmod +x preprovision.sh
   sudo ./preprovision.sh
   ```

2. **Run playbook:**
   ```bash
   sudo ansible-playbook -i inventory/localhost.yml playbooks/site.yml
   ```

Enter your sudo password when prompted.

## Running Claude with Local Model

To use Claude with a local model server, configure the following environment variables:

```bash
export ANTHROPIC_AUTH_TOKEN="local-key"
export ANTHROPIC_BASE_URL=http://localhost:3456
claude
```
