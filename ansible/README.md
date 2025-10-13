# Ansible Automation for Homelab

This directory contains the core Ansible automation for my Homelab. It's
structured using roles and layered playbooks to ensure modularity, reusability,
and maintainability.

## Project Structure

The project follows Ansible best practices for directory layout:

- **`inventory/`**: Contains the inventory files that define the hosts Ansible
  will manage. This is structured as a directory to easily support multiple
  environments (e.g., production, staging).
- **`group_vars/`**: Contains variables that apply to groups of hosts defined in
  the inventory. The `all.yaml` file holds variables that are global to the
  all hosts.
- **`roles/`**: Contains the self-contained, reusable components of the
  automation. Each role is responsible for managing a specific piece of software
  or configuration.
- **`requirements.yaml`**: Lists the Ansible Galaxy dependencies required by the
  playbooks.
- **`*.yaml`**: High-level playbooks that orchestrate the execution of roles
  against hosts defined in the inventory.

## Playbook Execution Order

The automation is designed to be run in a specific sequence, as the layers build upon each other.

1. **`proxmox.yaml`**: This playbook targets the Proxmox hosts directly. It's
   responsible for preparing the underlying hypervisor infrastructure, such as
   setting up an NFS server.
1. **`kubernetes.yaml`**: This playbook targets `localhost` and interacts with
   the Kubernetes API. It deploys cluster-level services that applications will
   depend on, such as the storage provisioner (`nfs_provisioner`) and the load
   balancer (`metallb`).
1. **`apps.yaml`**: This playbook also targets `localhost` and deploys the
   end-user applications, such as Jellyfin.

## Variable Management

- **Global Variables**: All user-configurable variables are centralized
  in `ansible/group_vars/all.yaml`. This file is the single source of truth for
  environment-specific settings like IP addresses and subnets.
- **Role Defaults**: Each role in the `roles/` directory that includes optional
  configuration contains a `defaults/main.yaml` file. This file defines the
  default configuration for that role. Any variable defined in
  `group_vars/all.yaml` will override these defaults.
- **Role Requirements**: Each role in the `roles/` directory includes its own
  `README.md` that defines the requirements for that role. Required variables
  that must be set in `group_vars/all.yaml` are clearly documented.

## Role Descriptions

- [**`cni_plugins`**][cni-plugins]: Installs and configure CNI plugins on the
  Kubernetes nodes to enable advanced networking for pods.
- [**`jellyfin`**][jellyfin]: Deploys the Jellyfin media server. This role
  manages the necessary Kubernetes resources, including `PersistentVolumeClaims`
  and `Services`, and deploys the application using the official Helm chart.
- [**`metallb`**][metallb]: Deploys MetalLB to the Kubernetes cluster, enabling
  `LoadBalancer` type services for bare-metal environments.
- [**`nfs_provisioner`**][nfs-provisioner]: Deploys the
  `nfs-subdir-external-provisioner` to the Kubernetes cluster. It sets up two
  `StorageClass` resources (`nfs-config` and `nfs-media`) to handle dynamic
  volume provisioning.
- [**`nfs_server`**][nfs-server]: Configures the Proxmox VE host as an NFS
  server, creating and exporting the necessary directories for Kubernetes
  persistent storage.

[cni-plugins]: roles/cni_plugins/README.md
[jellyfin]: roles/jellyfin/README.md
[metallb]: roles/metallb/README.md
[nfs-provisioner]: roles/nfs_provisioner/README.md
[nfs-server]: roles/nfs_server/README.md
