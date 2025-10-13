# Homelab Configuration

This repository contains the Configuration-as-Code (CaC) for my personal homelab
environment. It uses [Ansible] to automate the configuration of Proxmox hosts
and the deployment of applications onto a Talos Kubernetes cluster.

## Prerequisites

> [!NOTE]
> These instructions were developed and tested on Linux. MacOS should be
> similar, but steps may very slightly. Windows is not supported.

Before you begin, ensure you have the following installed on your local machine
(the Ansible control node):

- git: To clone this repository.
- [uv]: A fast Python package manager.
- SSH Access: SSH key-based authentication configured for the root user (or a
  user with passwordless sudo) on the Proxmox hosts.

## Getting Started

These steps will guide you through setting up the local environment and running
the playbooks.

1. Clone the Repository

   ```bash
    git clone git@github.com:jamesiarmes/homelab.git
    cd homelab
   ```

1. Set Up the Python Virtual Environment with `uv`.

   ```bash
   uv venv
   source .venv/bin/activate
   ```

1. Install Python and Ansible Dependencies.

   ```bash
    uv pip install -r requirements.txt
    ansible-galaxy install -r ansible/requirements.yaml
   ```

1. Configure Your Environment

   You must update the configuration variables to match your specific homelab setup.

   1. Configure the Inventory:

      - Edit `ansible/inventory/hosts.yaml`.
      - Set `ansible_host` to the IP address of your Proxmox server.

   1. Configure Global Variables:

      - Edit `ansible/group_vars/all.yaml`.
      - Update the variable values to match your network and desired
        configuration. Pay special attention to `proxmox_host_ip`,
        `k8s_node_subnet`, `metallb_ip_range`, and `jellyfin_lb_ip`.

1. Run the Ansible Playbooks

   The automation is divided into three logical playbooks that should be run in
   order. All commands should be run from the root of the homelab repository.

   1. Configure the Proxmox Host

      ```bash
      ansible-playbook -i ansible/inventory ansible/proxmox.yaml
      ```

   1. Configure Kubernetes Cluster Services

      ```bash
      ansible-playbook -i ansible/inventory ansible/kubernetes.yaml
      ```

   1. Deploy Applications

      ```bash
      ansible-playbook -i ansible/inventory ansible/apps.yaml
      ```

After completing these steps, the configured applications should be up and
running.

[ansible]: https://docs.ansible.com/
[uv]: https://github.com/astral-sh/uv
