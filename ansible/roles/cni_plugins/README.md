# Ansible Role: CNI Plugins

This role enables advanced networking features, such as attaching pods directly
to the physical LAN.

## Purpose

This role performs the following sequence of actions:

- Installs [Cilium]: Deploys Cilium via its Helm chart to act as the new primary
  CNI for the cluster. Cilium is a modern, high-performance CNI that uses
  [eBPF].
- Installs [Multus]: Deploys Multus CNI, a "meta-plugin" that allows pods to be
  attached to multiple network interfaces.
- Installs network drivers: Creates an init contiainer that ensures pods have
  the necessary network drivers installed.
- Configures [Macvlan]: Creates a `NetworkAttachmentDefinition` for a macvlan
  network. This allows pods to have a secondary network interface with its own
  MAC address, making it appear as a physical device on the local network,
  enabling broadcast and multicast for service discovery.

## Requirements

- A running Kubernetes cluster.
- `kubectl` configured with access to the cluster API.

## Role Variables

### Required Variables

This role requires the following variables to be defined, typically in
`group_vars/all.yaml`:

- `cni_plugins_subnet_cidr`: The CIDR notation for your local LAN subnet. This
  is used by the macvlan IPAM (IP Address Management) plugin to correctly
  configure the secondary network interfaces on your pods.

  - Example: `192.168.1.0/24`

### Default Variables

This role provides a set of optional variables with default values in
[`defaults/main.yaml`][defaults]. You can override any of these values in your
`group_vars` or `host_vars` to customize the deployment.

- `cni_plugins_macvlan_gateway`: The gateway IP address for your local LAN.
  This is used by the macvlan IPAM plugin to correctly configure the secondary
  network interfaces on your pods.

  - Example: `192.168.1.1`

- `cni_plugins_macvlan_interface`: The name of the primary physical network
  interface on the host that the Kubernetes node VMs are bridged to (e.g., eth0,
  enp6s0). This interface is used by macvlan as the parent to attach pod
  interfaces directly to the LAN.

  - Example: `eth0`

- `cni_plugins_macvlan_version`: The version of the Multus macvlan CNI plugin to
  install. This must be a valid, full semantic version string.

  - Example: `4.2.2`

[cilium]: https://github.com/cilium/cilium
[defaults]: defaults/main.yaml
[ebpf]: https://ebpf.io/
[macvlan]: https://docs.docker.com/engine/network/drivers/macvlan/
[multus]: https://github.com/k8snetworkplumbingwg/multus-cni
