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

This role provides a comprehensive set of default Helm chart values in
[`defaults/main.yaml`][defaults] under `jellyfin_helm_values`. You can override
any of these values in your `group_vars` or `host_vars` to customize the
deployment.

For example, to change the requested memory, you would add the following to
`group_vars/all.yaml`:

```yaml
jellyfin_helm_values:
  resources:
    requests:
      memory: 3Gi
```     

[defaults]: defaults/main.yaml
[helm]: https://github.com/jellyfin/jellyfin-helm
[jellyfin]: https://jellyfin.org/
