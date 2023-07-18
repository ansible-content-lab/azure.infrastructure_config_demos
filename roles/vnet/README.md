# VNET Role

This Ansible role contains repeatable automation for deploying and removing a VNET and a subnet on Azure.

## Create VNET

Creates a virtual network in an Azure resource group.

### Variables

The following variables are used during deployment and can be configured as extra vars at deploy time if you require something other than the defaults.

```yaml
---
region: eastus
resource_group: rg
route_table_name: route-table
vnet_name: vnet
vnet_cidr: 10.0.200.0/23
subnet_name: subnet
subnet_cidr: 10.0.200.0/24
```

## Delete VNET

The delete vm task removes VNET based on the resources created in the `create` task.

```yaml
---
resource_group: rh
vm_name: rhel9-test
nic_name: rhel9-test-nic
network_sec_group_name: rhel9-test-sg
public_ip_name: rhel9-test-public-ip
```
