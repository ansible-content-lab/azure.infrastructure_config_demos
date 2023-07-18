# Virtual Machine Role

This Ansible role contains repeatable automation for virtual machine management on Azure.

## Create VM

Creates a virtual machine in an existing Azure resource group attached to an existing VNET and subnet.

### Variables

The following variables are used during deployment and can be configured as extra vars at deploy time if you require something other than the defaults.

#### Linux Required

The following is an example of using the RHEL 9 blueprint with required fields.

```yaml
---
region: eastus
resource_group: rg
os_type: Linux
vm_name: rhel9-test
vm_blueprint: rhel9
nic_name: "{{ vm_name }}-nic"
network_sec_group_name: "{{ vm_name }}-sg"
public_ip_name: "{{ vm_name }}-public-ip"
vnet_name: vnet
subnet_name: subnet
admin_user: azureuser
admin_password: R3dH@t123456
ssh_public_keys:
  - path: "/home/{{ admin_user }}/.ssh/authorized_keys"
    key_data: "{{ lookup('file', '/home/runner/.ssh/id_rsa_azure_demo.pub') }}"
```

#### Windows Required

The following is an example of using the Windows Server 2022 blueprint with required fields.

```yaml
---
region: eastus
resource_group: rg
os_type: Windows
vm_name: win2022-test
vm_blueprint: windowsserver2022
nic_name: "{{ vm_name }}-nic"
network_sec_group_name: "{{ vm_name }}-sg"
public_ip_name: "{{ vm_name }}-public-ip"
vnet_name: vnet
subnet_name: subnet
admin_user: azureuser
admin_password: R3dH@t123456
```

#### Optional

```yaml
create_vm_priority: None #Spot
create_public_ip: true
```

## Delete VM

The delete vm task removes VMs based on the resources created in the `create` task.

```yaml
---
resource_group: rh
vm_name: rhel9-test
nic_name: rhel9-test-nic
network_sec_group_name: rhel9-test-sg
public_ip_name: rhel9-test-public-ip
```
