# Edunetes - Kubernetes self-study environment on the Ubuntu 22.04 laptop

A light-weight Kubernetes distribution for educational purposes. Run the latest Vanilla Kubernetes on a single workstation or laptop.

## Features

- Vanilla Kubernetes from pkgs.k8s.io repository
- Flannel or Calico CNI plugin
- MetalLB loadbalander
- one control plane and multiple worker nodes

### Options

- Headlamp
- ArgoCD

## Requirements

- KVM host on Ubuntu 22.04
- Python 3.10 virtual environment
- modern multithread computer 16gb memory for 2 nodes, 32gb memory for 4 nodes

[Setting up KVM host and installing Terraform](docs/SET-UP-KMV-HOST-UBUNTU-22.04.md)

## Cluster configuration files

[Configuration](group_vars/all/main.yaml)
[Configuration](group_vars/all/nodes.yaml)

## Nodes (Ubuntu 22.04)

Ansible playbook which creates KVM-based Ubuntu 22.04 nodes

    ansible-playbook playbook-cluster-create.yaml -c local -K

## Cluster configuration

Configure a Kubernetes cluster with following playbooks.

    ansible-playbook playbook-cluster-nodes-prepare.yaml --private-key=.ssh/id_edunetes -u kubeadmin
    ansible-playbook playbook-cluster-nodes-control-node-set-up.yaml --private-key=.ssh/id_edunetes -u kubeadmin
    ansible-playbook playbook-cluster-nodes-workers-join.yaml --private-key=.ssh/id_edunetes -u kubeadmin
    ansible-playbook playbook-cluster-finalize.yaml --private-key=.ssh/id_edunetes -u kubeadmin

After configuration, you can check the status of the cluster and pods with the following commands.

    kubectl get nodes -o wide
    NAME                             STATUS   ROLES           AGE     VERSION   INTERNAL-IP     EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION       CONTAINER-RUNTIME
    control-plane01.edunetes.local   Ready    control-plane   5m52s   v1.31.2   172.16.254.11   <none>        Ubuntu 22.04.5 LTS   5.15.0-119-generic   containerd://1.7.12
    worker01.edunetes.local          Ready    <none>          5m3s    v1.31.2   172.16.254.21   <none>        Ubuntu 22.04.5 LTS   5.15.0-119-generic   containerd://1.7.12
    worker02.edunetes.local          Ready    <none>          5m4s    v1.31.2   172.16.254.22   <none>        Ubuntu 22.04.5 LTS   5.15.0-119-generic   containerd://1.7.12
    worker03.edunetes.local          Ready    <none>          5m3s    v1.31.2   172.16.254.23   <none>        Ubuntu 22.04.5 LTS   5.15.0-119-generic   containerd://1.7.12

    kubectl get pods -A -o wide
    NAMESPACE            NAME                                                     READY   STATUS    RESTARTS        AGE     IP              NODE                             NOMINATED NODE   READINESS GATES
    kube-flannel         kube-flannel-ds-4bghc                                    1/1     Running   0               6m24s   172.16.254.23   worker03.edunetes.local          <none>           <none>
    kube-flannel         kube-flannel-ds-b4k2d                                    1/1     Running   0               7m5s    172.16.254.11   control-plane01.edunetes.local   <none>           <none>
    kube-flannel         kube-flannel-ds-hcq86                                    1/1     Running   0               6m24s   172.16.254.21   worker01.edunetes.local          <none>           <none>
    ...

## Removing cluster

    ansible-playbook -i inventory.yaml playbook-cluster-destroy.yaml -K
