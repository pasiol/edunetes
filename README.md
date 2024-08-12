# Edunetes - Kubernetes self-study environment on an Ubuntu 22.04 machine

Run the latest Kubernetes release on your Linux laptop.

## Nodes

Ansible playbook which create KVM-based Ubuntu 22.04 virtual machine nodes

    ansible-playbook playbook-kvm-nodes-create.yaml -c local -K

## Cluster configuration

 A vanilla Kubernetes cluster is installed on the nodes.

    ansible-playbook playbook-cluster-nodes-prepare.yaml --private-key=.ssh/id_edunetes -u kubeadmin
    ansible-playbook playbook-cluster-nodes-control-node-set-up.yaml --private-key=.ssh/id_edunetes -u kubeadmin
    ansible-playbook playbook-cluster-nodes-workers-join.yaml --private-key=.ssh/id_edunetes -u kubeadmin


All group variables are defined on groups_vars/all/main.yaml

## Requirements

KVM host on Ubuntu 22.04

Prepapre KVM host running following commands.

    ./set-up-ubuntu22.04-host.sh
    ansible-playbook set-up-kvm-playbook.yaml -K


Under development, on the Alpha stage