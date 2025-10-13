# Ansible Role: NFS Provisioner

This role deploys the `nfs-subdir-external-provisioner` to a Kubernetes cluster
using its official [Helm chart][helm].

## Purpose

This role sets up dynamic storage provisioning for Kubernetes using an external
NFS server. It deploys two separate instances of the provisioner to create a
tiered storage system:

- `nfs-config`: A StorageClass for application configuration data.
- `nfs-media`: A StorageClass for large, read-heavy media files.

## Requirements

- A running Kubernetes cluster.
- A configured `kubectl` on the Ansible control node.
- An existing NFS server that is accessible from the Kubernetes nodes.

## Role Variables

### Required Variables

This role requires the following variables to be defined, typically in
`group_vars/all.yaml`:

- `proxmox_host_ip`: The IP address of the external NFS server.
  - Example: `192.168.1.10`

[helm]: https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner
