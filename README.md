[![Validation CI](https://github.com/ansible-content-lab/azure.infrastructure_config_demos/actions/workflows/validate.yml/badge.svg)](https://github.com/ansible-content-lab/azure.infrastructure_config_demos/actions/workflows/validate.yml)

# Ansible Collection - azure.infrastructure_config_demos

This repository hosts the `azure.infrastructure_config_demos` Ansible Collection.

The collection includes a variety of Ansible roles and playbook to help automate the management of resources on Microsoft Azure.

This content was developed as part of the [Ansible Content Lab for Cloud Content](https://ansible-content-lab.github.io/), a program from the Ansible team to help incubate Ansible cloud use cases from ideation to collections and roles.

## Included Content

<!--start collection content-->
### Roles

Click on the role name to be directed to the README specifically for that role.

| Name                                                                                                                                                                                    | Description                                                                                                                                                                               |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [azure.infrastructure_config_demos.transit_peered_networks](https://github.com/ansible-content-lab/azure.infrastructure_config_demos/blob/main/roles/transit_peered_networks/README.md) | A role to create a hub-and-spoke VPC networking architecture that includes DMZ and private networks.                                                                                      |
| [azure.infrastructure_config_demos.log_analytics](https://github.com/ansible-content-lab/azure.infrastructure_config_demos/blob/main/roles/log_analytics/README.md)                     | A role that contains tasks to create and destroy an Azure Log Analytics workspace and then attach Linux-based VMs to the workspace by installing and configuring the Log Analytics agent. |
| [azure.infrastructure_config_demos.arc](https://github.com/ansible-content-lab/azure.infrastructure_config_demos/blob/main/roles/arc/README.md)                                         | A role that contains tasks for managing Arc-enabled servers such as installing the Azure agent and enabling Azure logging extensions.                                                     |
| [azure.infrastructure_config_demos.vms](https://github.com/ansible-content-lab/azure.infrastructure_config_demos/blob/main/roles/vms/README.md)                                         | A role for repeatable VM management tasks.                                                                                                                                                |

### Playbooks

| Name                                                | Role(s) Used                    | Description                                                                                                                                    |
| --------------------------------------------------- | ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `playbook_create_transit_network.yml`               | `roles.transit_peered_networks` | A playbook to create a multi-VPC hub-and-spoke network configuration using a transit gateway with DMZ and private networks.                    |
| `playbook_delete_transit_network.yml`               | `roles.transit_peered_networks` | Deletes AWS resources created in the `playbook_create_transit_network` playbook.                                                               |
| `playbook_create_resource_group.yml`                | N/A                             | Simple playbook demonstrating how to create an Azure resource group from extra vars.                                                           |
| `playbook_create_rhel_vm.yml`                       | N/A                             | Creates a RHEL VM with either a public or private IP based on extra vars.                                                                      |
| `playbook_create_windows_vm.yml`                    | N/A                             | Creates a Windows VM with either a public or private IP based on extra vars.                                                                   |
| `playbook_delete_windows_vm.yml`                    | N/A                             | Deletes Windows VM created with `playbook_create_windows_vm.yml`.                                                                              |
| `playbook_delete_resource_group.yml`                | N/A                             | Deletes a resource group and all resources within it.                                                                                          |
| `playbook_update_rhel_vms.yml`                      | N/A                             | Runs `dnf upgrade -y` on RHEL VMs.                                                                                                             |
| `playbook_create_log_analytics_workspace.yml`       | `role.log_analytics`            | Creates a Log Analytics workspace for storing log data with logging extensions.                                                                |
| `playbook_install_log_analytics_agent.yml`          | `roles.log_analytics`           | Deploys a Log Analytics workspace into your Azure subscription and then installs and configures Linux hosts to communicate with the workspace. |
| `playbook_uninstall_log_analytics_agent.yml`        | `roles.log_analytics`           | Uninstalls the Log Analytics agent on Linux hosts and then deletes the Log Analytics workspace from your Azure subscription.                   |
| `playbook_install_arc_agent.yml`                    | `roles.arc`                     | Installs the Azure Arc agent on inventory hosts.                                                                                               |
| `playbook_replace_log_analytics_with_arc_linux.yml` | `roles.arc`                     | Installs the Arc agent, deploys the Azure Monitoring extension, and removes the log analytics extension or agent on Linux hosts.               |
| `playbook_enable_arc_azure_monitor.yml`             | `roles.arc`                     | Enables the Azure Monitor extension on servers that have the Azure Arc agent installed.                                                        |
| `playbook_enable_arc_extensions.yml`                | `roles.arc`                     | Enables multiple Arc extensions at the same time.                                                                                              |
| `playbook_disable_arc_extensions.yml`               | `roles.arc`                     | Disables Arc extensions that are provided via extra vars.                                                                                      |
<!--end collection content-->

#### Create Network Playbooks

The `azure.infrastructure_config_demos.create_transit_network` playbook has another tasks block that will attempt to configure the VM resources deployed by the role a bit farther.  When the role completes, VM instances in the DMZ will still need to be configured with SSH configuration in order to communicate with VM instances in other VNETs.

To connect to the DMZ VM instance, the `ansible_ssh_private_key_file` variable needs to be set with the contents of the private key for SSH connections so that the machine running the playbook can connect to the newly created VM instance.  You may set this variable in any way that Ansible allows, i.e. extra var, host var, machine credential, etc.  It must be set or the configuration step will be skipped.  The `ansible_ssh_user` variable is set automatically to the user `azureuser` that is standard on AWS AMIs.

When the following variables are set, then the playbook will also configure the DMZ VM instance with the SSH config and a private key to communicate with each other.  This leaves the deployment in an immediately accessible state.

| Variable                                  | Use                                                                                                                |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| `ansible_ssh_private_key_file_local_path` | The local path to an SSH private key that will be copied to the bastion host.                                      |
| `ansible_ssh_private_key_file_dest_path`  | The destination path for the SSH key on the bastion host.                                                          |
| `priv_network_hosts_pattern`              | A hosts pattern that will be added to the bastion host SSH config for easier access to machines on other networks. |
| `priv_network_ssh_user`                   | The SSH user that will be configured in the bastion host SSH config.                                               |

```yaml
---
ansible_ssh_private_key_file_local_path: ~/.ssh/azure-ssh-key
ansible_ssh_private_key_file_dest_path: ~/.ssh/azure-ssh-key
priv_network_hosts_pattern: 172.*
priv_network_ssh_user: azureuser
```

## Installation and Usage

### Ansible Automation Controller or AWX

#### Automated

It is possible to use Ansible to configure Ansible Automation Platform so that your Ansible Automation Platform configuration is replicable, backed-up, and change sets can be referenced as git commits.  The [aoc.controller_demo_config](https://github.com/ansible-content-lab/aoc.controller_demo_config) project demonstrates how to use the Ansible CLI to configure all of the components of Ansible Automation Platform and can assist with recreating individual templates and workflows rapidly across Ansible Automation Platform environments.

#### Manually

Ensure that you are logged in to Ansible Automation Controller before proceeding with these steps.

##### Create the Project

1. Click `Projects` in the left menu.
2. Click the `Add` button.
3. Enter the following fields:
   * Name: `Ansible Cloud Content Lab - Azure Roles`
   * Organization: `Default` (or your preferred organization)
   * Execution Environment: `Cloud Services Execution Environment` (if using Ansible on Azure)
     * If you need an execution environment with cloud collections, you can build your own through [community examples](https://github.com/scottharwell/cloud-ee) or use [pre-built](https://quay.io/repository/scottharwell/cloud-ee) (but unsupported) examples.
   * Source Control Type: `Git`
   * Source Control URL: `https://github.com/ansible-content-lab/azure.infrastructure_config_demos.git`
4. Click the `Save` button.

##### Create Automation Templates

Each automation template is equivalent to a playbook in this repository.  Repeat these steps for each template/playbook that you want to use and change the variables specific to the individual playbook.

1. Click `Templates` in the left menu.
2. Click the `Add` button.
3. Ensure the following fields:
   * Name: `lab.azure_roles.create_rhel_vm` (Any name will work -- use a convention that you can remember for template naming)
   * Job Type: `Run`
   * Inventory: `localhost` (You must have an inventory created with `localhost` as the host)
   * Project: `Ansible Cloud Content Lab - Azure Roles`
   * Playbook: `playbooks/create_rhel_vm.yml` (Use the name of the playbook in this collection)
   * Credentials: `Azure Service Principal` (You must have an Azure Service Principal credential in order to use the Azure collection)
   * Variables: (Will differ per playbook)

    ```yaml
    ---
    resource_group: resource-group
    region: eastus
    vm_name: create-rhel-test
    nic_name: create-rhel-test-nic
    network_sec_group_name: create-rhel-test-sg
    public_ip_name: create-rhel-test-public-ip
    vnet_name: your-existing-vnet
    subnet_name: your-existing-subnet
    create_public_ip: true
    ssh_public_key: "" # Copy an RSA (Azure required) public key for VM access
    admin_password: ChangeMe # Set to a secure password
    ```

4. Click the `Save` button.

You can now run the automation template by clicking the `Launch` button.

### Local Installation (Ansible Galaxy CLI)

Before using the this collection, you need to install it with the Ansible Galaxy CLI:

`ansible-galaxy collection install git+https://github.com/ansible-content-lab/azure.infrastructure_config_demos.git`

You can also include it in a `requirements.yml` file and install it via `ansible-galaxy collection install -r requirements.yml`, using the format:

```yaml
---
collections:
  - name: https://github.com/ansible-content-lab/azure.infrastructure_config_demos.git
    type: git
    version: main
```

## Adding a Git Pre-Commit Hook

This repo includes a configuration file for `ansible-lint` to be run as a git [pre-commit](https://pre-commit.com/) hook. To install the hook, run `pre-commit install` from the root directory of this repo once you have the pre-commit utility installed on your machine.

## License

GNU General Public License v3.0 or later

See [LICENSE](https://github.com/ansible-content-lab/lab.aws_roles/blob/main/LICENSE) to see the full text.
