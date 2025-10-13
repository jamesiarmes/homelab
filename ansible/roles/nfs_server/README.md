# Ansible Role: NFS Server

This role configures a set of NFS shares and exports as specified by
`nfs_shares`.

## Purpose

### This role performs the following actions:

- Installs `the nfs-kernel-server` package.
- Creates directories under `/export` to be used as NFS shares.
- Sets the ownership and permissions on the share directories to
  `nobody:nogroup` and `777` to allow access from the Kubernetes NFS
  provisioner.
- Configures `/etc/exports` to share these directories with the Kubernetes nodes.
- Restarts the NFS service to apply the changes.

## Requirements

- The target host must be a Debian-based system (e.g., Proxmox VE).
- The control node must have SSH access with `become` (sudo) privileges to the
  target host.

## Role Variables

### Required Variables

This role requires the following variables to be defined, typically in
`group_vars/all.yaml`:

- `nfs_clients`: The network address of allowed client(s) in CIDR notation. This
  is used to restrict access to the NFS shares.
  - Example: `192.168.1.0/24`
- `nfs_shares`: A list of directory names to be created under `/export` and
  shared via NFS.
  - Example: `['k8s-pv-1', 'k8s-pv-2']`
