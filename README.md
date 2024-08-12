# Edunetes - Kubernetes self-study environment on the Ubuntu 22.04 laptop

Run the latest Vanilla Kubernetes release on your Linux laptop.

## Cluster configuration

[Configuration](group_vars/all/main.yaml)

## Nodes

Ansible playbook, which creates KVM-based Ubuntu 22.04 virtual machine nodes

    ansible-playbook playbook-kvm-nodes-create.yaml -c local -K

## Cluster configuration

Configure a Kubernetes cluster on the nodes.

    ansible-playbook playbook-cluster-nodes-prepare.yaml --private-key=.ssh/id_edunetes -u kubeadmin
    ansible-playbook playbook-cluster-nodes-control-node-set-up.yaml --private-key=.ssh/id_edunetes -u kubeadmin
    ansible-playbook playbook-cluster-nodes-workers-join.yaml --private-key=.ssh/id_edunetes -u kubeadmin

## Requirements

- KVM host on Ubuntu 22.04
- Python 3.10 virtual environment
- modern multithread laptop 16gb memory for 2 nodes, 32gb memory for 4 nodes
- one control plane is supported

Prepare KVM host running the following commands.

    ./set-up-ubuntu22.04-host.sh
    git clone https://github.com/pasiol/edunetes.git
    cd edunetes
    python3.10 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt
    ansible-galaxy install -r requirements.yaml
    ansible-playbook playbook-kvm-host-set-up.yaml -K


Under development, on the Alpha stage