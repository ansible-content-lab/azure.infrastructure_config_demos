# VM to Azure Prep Role

This Ansible role contains Ansible automation to prep local virtual machines (on-prem or not on Azure) with the packages and OS configuration necessary to export the virtual machine and then run it on Azure.

This role is only responsible for the VM preparation.  It does not handle the work of actually moving the machine image to Azure.

It is recommended that you run this role on VMs that are cloned from production instances and not directly against production VMs since it alters boot settings, kernel drivers, and installs packages required by Azure.

## Using the Role

This role is always intended to be called using `main.yml`.  This file will subsequently determine which operations to perform based on the OS family of the target virtual machine.

Currently, this role supports the following OS families:

* `RedHat`

### Variables

The following optional variables may be set to perform tasks that are not required, but recommended, before packaging a VM image for Azure.

#### Optional

```yaml
---
# User accounts to remove
vm_to_azure_prep_users:
  - scott
  - hicham
  - matthew
# Ethernet connections to remove
vm_to_azure_network_connections_to_remove:
  - System eth0
```
