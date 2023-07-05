# Virtual Machine Role

This Ansible role contains repeatable automation for virtual machine management on Azure.

## Create RHEL VM

Creates an example RHEL virtual machine in an existing Azure resource group attached to an existing VNET and subnet.

### Variables

The following variables are used during deployment and can be configured as extra vars at deploy time if you require something other than the defaults.

#### Required

```yaml
---
region: eastus
resource_group: existing_resource_group
nic_name: new_nic_name
network_sec_group_name: new_security_group_name
public_ip_name: new_public_ip_name
vnet_name: existing_vnet_name
subnet_name: existing_subnet_name
create_public_ip: false
vm_name: name_vnet_name
ssh_public_key: <contents of ssh public key>
```

#### Optional

```yaml
---
vm_size: Standard_DS3_v2
disk_type: Standard_LRS
admin_user: azureuser
rhel_vm_sku: "8_6"
```

## Create Windows VM

Creates an example Windows Server virtual machine in an existing Azure resource group attached to an existing VNET and subnet.

### Variables

The following variables are used during deployment and can be configured as extra vars at deploy time if you require something other than the defaults.

#### Required

```yaml
---
region: eastus
resource_group: existing_resource_group
nic_name: new_nic_name
network_sec_group_name: new_security_group_name
public_ip_name: new_public_ip_name
vnet_name: existing_vnet_name
subnet_name: existing_subnet_name
create_public_ip: false
vm_name: name_vnet_name
admin_password: yoursecretpassword
```

#### Optional

```yaml
---
vm_size: Standard_DS3_v2
disk_type: Standard_LRS
admin_user: azureuser
windows_vm_sku: 2022-Datacenter
```
