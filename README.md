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
   sudo ansible-playbook -i inventory/localhost.yml playbooks/site.yml --ask-become-pass
   ```

Enter your sudo password when prompted.