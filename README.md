# Proxmox

## Initial configuration

- Remove the `local-lvm` storage in the console. We need to add storage to the `local` storage.

1. Click on datacenter
2. Click on left menu "Storage"
3. Select local-lvm and delete.

- Connect to the node main (node -> Shell).

```bash
lvremove /dev/pve/data
lvresize -l +100%FREE /dev/pve/root
resize2fs /dev/mapper/pve-root
```

Now we need to make available the local storage unit.
- Click on datacenter.
- Click on Storage (left menu).
- Click on local.
- Click on edit.
- Select which content are allowed to use this storage.
  - Ensure "Disk image" is selected


## Upload ISOs

- Click on local storage
- Click on ISO images (left menu)
- Click on upload, select the pre-downloaded ISO

## Create VMS

- Click on "Create VM"

### General Tab

- Choose an ID
- Give it a name

### OS Tab

- Choose your storage (local)
- Choose your ISO image

### System Tab

...

### Hard Disk Tab

- Give it a storage size

### CPU Tab

- Select the number of cores. No need to touch anything else.

### Memory tab

- Select how many RAM you need.

### Network tab

...


# Debian

1. install the bareminimum (SSH server and utils, no desktop environment por favor)
2. login as root
3. run the following:

```bash
apt update
apt upgrade
apt install sudo curl
usermod -aG sudo <user>
```
