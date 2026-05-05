# Ansible Role: media_storage

Configures an external hard drive on the Proxmox host for Jellyfin media: mounts
the drive at `/mnt/media`, optionally migrates existing data from
`/export/jellyfin-media` (or `/exports/jellyfin-media`), and bind-mounts the
drive's `jellyfin-media` directory at `/export/jellyfin-media` so NFS and
Kubernetes keep using the same path.

## Requirements

- Target host is Proxmox (or any Debian-based system with NFS server).
- The external drive is attached and (if using `media_storage_device`) already
  partitioned and formatted (e.g. XFS: `mkfs.xfs /dev/sdb1`).

## Role Variables

Set in `group_vars` or `host_vars` for the host that has the external drive:

- **`media_storage_device`** (optional if `media_storage_uuid` is set): Block
  device for the partition to mount (e.g. `"/dev/sdb1"`). Used for mounting and
  to read UUID for fstab when `media_storage_uuid` is not set.
- **`media_storage_uuid`** (optional): UUID of the partition. When set, used in
  fstab instead of the device path for stable mounting.
- **`media_storage_mount_point`**: Where to mount the drive (default:
  `"/mnt/media"`).
- **`media_storage_fstype`**: Filesystem type (default: `"xfs"`; good for large
  media files and streaming).

When neither `media_storage_device` nor `media_storage_uuid` is set, the role
skips all tasks (suitable for hosts without the drive).

## Example

In `host_vars/proxmox.yaml` or `group_vars/all.yaml`:

```yaml
media_storage_device: "/dev/sda1"
```

## Dependencies

- Runs after the `nfs_server` role so `/export` and NFS exports exist. The role
  ensures `/export` exists and creates `/export/jellyfin-media` before applying
  the bind mount.
