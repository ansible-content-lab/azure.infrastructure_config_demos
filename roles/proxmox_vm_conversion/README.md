# Proxmox VM Conversion Role

This Ansible role contains automation that will take a running Proxmox virtual machine, convert its disk image to an Azure-compatible disk image, and then move the created disk image to localhost.  The following steps occur in the order listed.

1. Shutdown the running VM.
2. Create a VHD disk image from the Proxmox VM disk image using the `qemu-img` utility on the Proxmox server.
3. Copy the VHD disk image to localhost.  This last step is required because it's unlikely that the Proxmox host has Azure libraries that other automation would use to upload the image to Azure.
4. Delete VHD disk image on the Proxmox host (if variable is set to cleanup).
5. Restart the VM.

## Using the Role

This role is always intended to be called using `main.yml`.  This file runs a serialized set of operations to shutdown a VM, use `qemu-img` to convert the VM to an Azure compatible format, and then move the new image to localhost.

### Variables

The following variables may be set in order to configure both storage and VM settings for the process.

#### Required

```yaml
---
proxmox_vm_migration_source_format: qcow2
proxmox_vm_migration_image_src: /mnt/pve/local/images/108/vm-108-disk-1.qcow2
proxmox_vm_migration_image_filename: vm-108-disk-1.vhd
proxmox_vm_migration_image_dest: "/mnt/pve/local/images/108/{{ proxmox_vm_migration_image_filename }}"
proxmox_vm_id: "108"
proxmox_api_user: api@pve
proxmox_api_token: token
proxmox_api_secret: secret
proxmox_api_host: proxmox.my.domain
```

#### Optional

Optional parameters are still *required*, but defaults are provided using these example values making them optional.

```yaml
---
proxmox_vm_migration_run_if_local_image_exists: true
proxmox_vm_migration_delete_proxmox_images_after_migration: true
proxmox_vm_migration_start_proxmox_vm: true
proxmox_vm_migration_local_image_path: /tmp/
```
