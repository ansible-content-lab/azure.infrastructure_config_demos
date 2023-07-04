# VM to Azure Role

This Ansible role contains automation that will take an Azure-prepared virtual machine disk image, then upload it to Azure blob storage, and then create a virtual machine from the disk image.

This role is only responsible moving a previously prepared VM image in `VHD` format to Azure.  It does not handle the work of preparing the virtual machine image.

## Using the Role

This role is always intended to be called using `main.yml`.  This file runs a serialized set of operations to push a local VM image to Azure blob storage and then create a running VM from the image.

The following Azure resources must exist prior to using this role:

* A resource group
* A storage account within the resource group
* A storage container within the storage account

### Variables

The following variables may be set in order to configure both storage and VM settings for the process.

#### Required

```yaml
---
vm_to_azure_file_name: local-image-file-name.vhd
vm_to_azure_resource_group: blob-storage-rg
vm_to_azure_storage_account: blob-storage-account
vm_to_azure_storage_container: blob-storage-container
vm_to_azure_image_name: vm-image-name
vm_to_azure_name: vm-name
vm_to_azure_vnet_resource_group: vm-vnet-rg
vm_to_azure_vnet_name: vm-vnet
vm_to_azure_subnet_name: vm-subnet
vm_to_azure_password: adminpassword
```

#### Optional

Optional parameters are still *required*, but defaults are provided using these example values making them optional.

```yaml
---
vm_to_azure_local_image_path: /tmp/
vm_to_azure_size: Standard_B1s
vm_to_azure_username: azureuser
```
