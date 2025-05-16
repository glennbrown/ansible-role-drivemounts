# Ansible Role: Drive Mounts

This role configures and optionally mounts drives on Debian-based Linux systems by managing entries in the `/etc/fstab` file.

## Requirements

- Ansible 2.9 or later
- Debian-based Linux system

## Role Variables

### Main Configuration Variables

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| automount_drives | Whether to automatically mount the drives or just configure them in fstab | No | false |
| drive_mounts | List of drive definitions | Yes | [] |

### Drive Configuration Structure

The role uses a list variable `drive_mounts` to configure all mount points. Each item in the list is a dictionary containing the disk identifier and its configuration.

```yaml
drive_mounts:
  - name: disk1
    mountpoint: "/mnt/disk1"
    device: "/dev/disk/by-id/ata-WDC_WD180EDGZ-11B9PA0_2TGGDS5Z-part1"
    fstype: "btrfs"
    options: 
      - "subvol=data"
```

Each drive definition can have the following properties:

| Property | Description | Required | Default |
|----------|-------------|----------|---------|
| name | Identifier for the disk (e.g., disk1, backup) | Yes | - |
| mountpoint | Mount path for the drive | Yes | - |
| device | The device to mount (path, UUID, LABEL, etc.) | Yes | - |
| fstype | The filesystem type | Yes | - |
| options | List of mount options | No | ["defaults"] |

## Example Playbook

```yaml
- hosts: servers
  vars:
    automount_drives: true  # Set to false to only configure fstab without mounting
    
    drive_mounts:
      - name: disk1
        mountpoint: "/mnt/disk1"
        device: "/dev/disk/by-id/ata-WDC_WD180EDGZ-11B9PA0_2TGGDS5Z-part1"
        fstype: "btrfs"
        options: 
          - "subvol=data"
      
      - name: backup
        mountpoint: "/backup"
        device: "/dev/sdb1"
        fstype: "ext4"
        options:
          - "defaults"
          - "noatime"
  
  roles:
    - ansible-role-drivemounts
```

## License

MIT

## Author Information

This role was created by the IB Team.