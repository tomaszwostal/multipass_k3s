# Ansible Role: multipass_k3s
[![Build Status](https://travis-ci.com/tomaszwostal/multipass_k3s.svg?branch=master)](https://travis-ci.com/tomaszwostal/multipass_k3s)

An ansible role that installs k3s kubernetes distribution in multipass virtual machines.

## Requirements
Debian based Linux distribution. Tested on Debian Bullseye.

## Role variables
By default k3s is installed without load balancer. It can be changed by ```install_k3s_exec``` variable.


Full list of variables:
```
k3s_main_name: "k3s-main"
k3s_main_number: 1
k3s_main_memory: 2048M
k3s_main_cpu: 1
k3s_main_disk: 25G

k3s_worker_name: "k3s-worker"
k3s_worker_number: 2
k3s_worker_memory: 4096M
k3s_worker_cpu: 2
k3s_worker_disk: 25G

k3s_kubeconfig_mode: 644
install_k3s_exec: '--no-deploy servicelb --no-deploy traefik'
```

## Dependencies
None.

## Example playbook
```
---
- hosts: supervisor
  gather_facts: yes

  tasks:
    - name: packages
      include_role:
        name: tomaszwostal.multipass_k3s
        tasks_from: packages

    - name: vms
      include_role:
        name: tomaszwostal.multipass_k3s
        tasks_from: vms

- hosts: k3s_masters
  gather_facts: yes

  tasks:
    - name: install k3s
      include_role:
        name: tomaszwostal.multipass_k3s
        tasks_from: k3s_main

- hosts: k3s_workers
  gather_facts: yes

  tasks:
    - name: install k3s
      include_role:
        name: tomaszwostal.multipass_k3s
        tasks_from: k3s_worker
```
## TODO
- [ ] multi master setup
- [ ] add teardown option
- [ ] add template for /etc/hosts file
- [ ] add support for Ubuntu hosts
- [ ] add support for RedHat/CentOS/Fedora hosts
- [ ] configure path for kubeconfig file