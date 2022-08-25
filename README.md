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
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [azure.infrastructure_config_demos.transit_peered_networks](https://github.com/ansible-content-lab/azure.infrastructure_config_demos/blob/main/roles/transit_peered_networks/README.md) | A role to create a hub-and-spoke VPC networking architecture that includes DMZ and private networks.                                                                                      |
| [azure.infrastructure_config_demos.log_analytics](https://github.com/ansible-content-lab/azure.infrastructure_config_demos/blob/main/roles/log_analytics/README.md)                     | A role that contains tasks to create and destroy an Azure Log Analytics workspace and then attach Linux-based VMs to the workspace by installing and configuring the Log Analytics agent. |

### Playbooks

| Name                                                            | Role(s) Used                    | Description                                                                                                                                    |
|-----------------------------------------------------------------|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| `azure.infrastructure_config_demos.create_transit_network`      | `roles.transit_peered_networks` | A playbook to create a multi-VPC hub-and-spoke network configuration using a transit gateway with DMZ and private networks.                    |
| `azure.infrastructure_config_demos.delete_transit_network`      | `roles.transit_peered_networks` | Deletes AWS resources created in the `create_transit_network` playbook.                                                                        |
| `azure.infrastructure_config_demos.create_resource_group.yml`   | N/A                             | Simple playbook demonstrating how to create an Azure resource group from extra vars.                                                           |
| `azure.infrastructure_config_demos.create_rhel_vm.yml`          | N/A                             | Creates a RHEL VM with either a public or private IP based on extra vars.                                                                      |
| `azure.infrastructure_config_demos.create_windows_vm.yml`       | N/A                             | Creates a Windows VM with either a public or private IP based on extra vars.                                                                   |
| `azure.infrastructure_config_demos.delete_resource_group.yml`   | N/A                             | Deletes a resource group and all resources within it.                                                                                          |
| `azure.infrastructure_config_demos.update_rhel_vms.yml`         | N/A                             | Runs `dnf upgrade -y` on RHEL VMs.                                                                                                             |
| `azure.infrastructure_config_demos.install_log_analytics.yml`   | `roles.log_analytics`           | Deploys a Log Analytics workspace into your Azure subscription and then installs and configures Linux hosts to communicate with the workspace. |
| `azure.infrastructure_config_demos.uninstall_log_analytics.yml` | `roles.log_analytics`           | Uninstalls the Log Analytics agent on Linux hosts and then deletes the Log Analytics workspace from your Azure subscription.                   |

<!--end collection content-->

#### Create Network Playbooks

The `azure.infrastructure_config_demos.create_transit_network` playbook has another tasks block that will attempt to configure the VM resources deployed by the role a bit farther.  When the role completes, VM instances in the DMZ will still need to be configured with SSH configuration in order to communicate with VM instances in other VNETs.

To connect to the DMZ VM instance, the `ansible_ssh_private_key_file` variable needs to be set with the contents of the private key for SSH connections so that the machine running the playbook can connect to the newly created VM instance.  You may set this variable in any way that Ansible allows, i.e. extra var, host var, machine credential, etc.  It must be set or the configuration step will be skipped.  The `ansible_ssh_user` variable is set automatically to the user `azureuser` that is standard on AWS AMIs.

When the following variables are set, then the playbook will also configure the DMZ VM instance with the SSH config and a private key to communicate with each other.  This leaves the deployment in an immediately accessible state.

| Variable                                  | Use                                                                                                                |
|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
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

### Installing the Collection with the Ansible Galaxy CLI

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

### Adding a Git Pre-Commit Hook

This repo includes a configuration file for `ansible-lint` to be run as a git [pre-commit](https://pre-commit.com/) hook. To install the hook, run `pre-commit install` from the root directory of this repo once you have the pre-commit utility installed on your machine.

# License
GNU General Public License v3.0 or later

See [LICENCE](https://github.com/ansible-content-lab/lab.aws_roles/blob/main/LICENSE) to see the full text.