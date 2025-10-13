# Ansible Role: MetalLB

This role installs and configures [MetalLB] on a bare-metal Kubernetes cluster to
provide `LoadBalancer` services.

## Purpose

This role automates the deployment of MetalLB in Layer 2 mode. It performs the
following actions:

- Applies the official MetalLB installation manifest.
- Waits for the MetalLB controller to become ready to ensure the cluster can
  accept its custom resources.
- Creates an `IPAddressPool` custom resource to define the range of IP addresses
  MetalLB can manage.
- Creates an `L2Advertisement` custom resource to instruct MetalLB to announce
  the service IPs on the local network.

## Requirements

- A running bare-metal Kubernetes cluster (or a virtualized one not on a cloud
  provider).
- A configured `kubectl` on the Ansible control node.

## Role Variables

### Required Variables

This role requires the following variables to be defined, typically in
`group_vars/all.yaml`:

- `metallb_ip_range`: The range of IP addresses for MetalLB to allocate. This
  range must be on the same subnet as your Kubernetes nodes but must NOT overlap
  with your router's DHCP pool.
  - Example: `192.168.1.240-192.168.1.250`

[metallb]: https://metallb.io/
