# Edunetes - Kubernetes self-study environment on the Ubuntu 22.04 laptop

Run the latest release of Vanilla Kubernetes on your Linux laptop.

## Cluster configuration

[Configuration](group_vars/all/main.yaml)
[Configuration](group_vars/all/nodes.yaml)

## Nodes

Ansible playbook, which creates KVM-based Ubuntu 22.04 virtual machine nodes

    ansible-playbook playbook-cluster-create.yaml -c local -K

## Cluster configuration

Configure a Kubernetes cluster on the nodes.

    ansible-playbook playbook-cluster-nodes-prepare.yaml --private-key=.ssh/id_edunetes -u kubeadmin
    ansible-playbook playbook-cluster-nodes-control-node-set-up.yaml --private-key=.ssh/id_edunetes -u kubeadmin
    ansible-playbook playbook-cluster-nodes-workers-join.yaml --private-key=.ssh/id_edunetes -u kubeadmin
    ansible-playbook playbook-cluster-finalize.yaml --private-key=.ssh/id_edunetes -u kubeadmin

## Removing cluster

    ansible-playbook -i inventory.yaml playbook-cluster-destroy.yaml -K

## Requirements

- KVM host on Ubuntu 22.04
- Python 3.10 virtual environment
- modern multithread laptop 16gb memory for 2 nodes, 32gb memory for 4 nodes

Prepare KVM host running the following commands.

    ./set-up-ubuntu22.04-host.sh
    git clone https://github.com/pasiol/edunetes.git
    cd edunetes
    python3.10 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt
    ansible-galaxy install -r requirements.yaml
    ansible-playbook playbook-kvm-host-set-up.yaml -K

## Features

- Vanilla Kubernetes from pkgs.k8s.io repository
- Flannel or Calico CNI plugin
- MetalLB loadbalander
- one control plane and multiple worker nodes

Under development, on the Alpha stage
