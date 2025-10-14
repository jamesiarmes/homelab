# Ansible Role: Jellyfin

This role deploys the [Jellyfin] media server to a Kubernetes cluster using the
[official Helm chart][helm].

## Requirements

This role assumes the following have already been configured on the Kubernetes
cluster:

-  A functional NFS storage backend with two `StorageClass` objects named
   `nfs-config` and `nfs-media`.
-  MetalLB installed and configured to provide `LoadBalancer` services.

## Role Variables

### Required Variables

This role requires the following variables to be defined, typically in
`group_vars/all.yaml`:

- `jellyfin_lb_ip`: The static IP address to assign to the Jellyfin
  `LoadBalancer` services. This IP must be within a range managed by MetalLB.

### Default Variables

### Default Variables

This role provides a set of optional variables with default values in
[`defaults/main.yaml`][defaults]. You can override any of these values in your
`group_vars` or `host_vars` to customize the deployment.

- `jellyfin_lb_port`: The port on which the Jellyfin service will be exposed.
  
  - Example: `8096`

- `jellyfin_published_url`: Optional URL to publish in service discovery.

  - Example: `http://jellyfin.example.com:8096`

- `jellyfin_version`: The version of the Jellyfin Docker image to deploy.
  
  - Example: `10.10.0`

[defaults]: defaults/main.yaml
[helm]: https://github.com/jellyfin/jellyfin-helm
[jellyfin]: https://jellyfin.org/
