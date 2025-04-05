# Ansible Role: Azure DevOps Agent

[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-MBH999.azure__devops__agent-blue.svg)](https://galaxy.ansible.com/MBH999/azure_devops_agent)

This Ansible role automates the deployment of an Azure DevOps build agent by:
1. Creating an Azure VM with all necessary networking components
2. Installing and configuring the Azure DevOps agent on the VM

## Requirements

- Ansible 2.9 or higher
- Azure subscription and credentials configured
- Azure CLI installed and authenticated
- Python packages:
  - azure-cli
  - azure-mgmt-core
  - azure-identity
- SSH key pair for VM authentication

## Role Variables

### Required Variables

```yaml
# Azure DevOps Configuration
vsts_url: ""        # Your Azure DevOps organization URL (e.g., https://dev.azure.com/your-org)
vsts_pat: ""        # Personal Access Token with Agent Pool management permissions
vsts_pool: ""       # Agent pool name where the agent will be registered
```

### Optional Variables

```yaml
# Azure VM Configuration
azure_vm_name: "devops-agent"              # Name of the VM
azure_vm_size: "Standard_B2s"              # VM size
azure_vm_username: "azureuser"             # VM admin username
azure_resource_group: "rg-devops-agent"    # Resource group name
azure_location: "uksouth"                  # Azure region
azure_vm_state: "present"                  # State of the VM (present/absent)

# Azure VM Image
azure_image_publisher: "Canonical"
azure_image_offer: "0001-com-ubuntu-server-focal"
azure_image_sku: "20_04-lts"
azure_image_version: "latest"

# DevOps Agent Configuration
vsts_agent_version: "3.234.0"              # Version of the Azure DevOps agent to install
vsts_agent_directory: "/opt/azure-devops-agent"  # Installation directory
```

## Dependencies

- azure.azcollection >= 1.10.0

## Example Playbook

```yaml
- hosts: localhost
  connection: local
  gather_facts: false
  vars:
    azure_vm_name: "my-devops-agent"
    azure_resource_group: "my-resource-group"
    azure_location: "eastus"
    vsts_url: "https://dev.azure.com/your-org"
    vsts_pat: "your-pat-token"
    vsts_pool: "Default"
  roles:
    - role: MBH999.azure_devops_agent
```

## Usage

1. Install the role:
   ```bash
   ansible-galaxy install MBH999.azure_devops_agent
   ```

2. Create a playbook (e.g., `deploy-agent.yml`) with your configuration:
   ```yaml
   - hosts: localhost
     roles:
       - role: MBH999.azure_devops_agent
     vars:
       vsts_url: "https://dev.azure.com/your-org"
       vsts_pat: "your-pat-token"
       vsts_pool: "Default"
   ```

3. Run the playbook:
   ```bash
   ansible-playbook deploy-agent.yml
   ```

## Security Considerations

1. Store sensitive variables (like `vsts_pat`) in an encrypted vault
2. Use SSH key authentication for VM access (password authentication is disabled)
3. Ensure your Azure DevOps PAT has minimal required permissions
4. Follow Azure security best practices for network security groups

## Troubleshooting

Common issues and solutions:

1. **VM Creation Fails**:
   - Verify Azure credentials and subscription
   - Check resource quota limits
   - Ensure VM size is available in selected region

2. **Agent Registration Fails**:
   - Verify PAT token permissions and expiration
   - Check network connectivity to Azure DevOps
   - Verify agent pool exists and is accessible

3. **SSH Connection Issues**:
   - Ensure SSH key pair is properly configured
   - Check network security group rules
   - Verify VM is fully provisioned

## License

MIT

## Author Information

This role was created by MBH999.

## Contributing

Issues and pull requests are welcome on GitHub at https://github.com/MBH999/ansible-role-azure-devops-agent 