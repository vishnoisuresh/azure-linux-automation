# Azure Linux Automation with Ansible

This repository contains a modular, production-ready Ansible workflow for provisioning RHEL Virtual Machines on Microsoft Azure.

## Repository Structure

- `playbooks/`: Individual modular steps for infrastructure provisioning.
- `inventory/`: Static inventory defining the local API target.
- `group_vars/`: Default configuration variables.
- `ansible.cfg`: Pre-configured Ansible environment settings.

## Getting Started

### 1. Prerequisites
- Azure CLI installed and authenticated (`az login`).
- Ansible and `azure.azcollection` installed.
- SSH Public Key for the VM.

### 2. Configuration
Create a local `env/extravars` file (ignored by Git) to override defaults:

```yaml
resource_group_name: "My-Customer-RG"
region: "westus2"
rhel_public_key: "ssh-rsa AAAAB3Nza... user@host"
```

### 3. Execution (The Workflow)

Run the playbooks in sequence to build the environment:

1. **Resource Group**: `ansible-playbook playbooks/01-resource-group.yml -e "@env/extravars"`
2. **VNet/Subnet**: `ansible-playbook playbooks/02-vnet.yml -e "@env/extravars"`
3. **Public IP**: `ansible-playbook playbooks/04-public-ip.yml -e "@env/extravars"`
4. **Security**: `ansible-playbook playbooks/05-nsg.yml -e "@env/extravars"`
5. **Compute**: `ansible-playbook playbooks/07-rhel-vm.yml -e "@env/extravars"`

## Security Best Practices
- **Service Principals**: Use a Service Principal for non-interactive automation.
- **Ansible Vault**: Encrypt sensitive variables like `subscription_id` or passwords using `ansible-vault`.
- **Key-Based Auth**: SSH passwords are disabled by default in these playbooks in favor of SSH keys.
