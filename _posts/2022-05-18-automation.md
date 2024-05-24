---
layout: post
title: Automation with Linux
subtitle: Ansible & Playbook
tags: [Automation]
last-updated: 2022-05-18
readtime: true
comments: true
---

Ansible is an open-source tool for automating Linux stuff


***

## Ansible
It helps administrators configure their servers with one click. For example, the admin wants to change DNS settings in every single Linux server.

Example: `/etc/resolve.conf`
```sh
nameserver 10.0.80.11
nameserver 10.0.80.12
```

By using Ansible, the admin only need to change the setting on the master server.

<br/>



## Get Started

{: .box-note}
**Demo**  - using three CentOS 7 VM

### Step 1: Install Ansible
1. Enable EPEL repository: `yum install epel-release -y`
2. Install Ansible: `yum install ansible -y`

### Step 2: Add hosts

Add hosts to your master server

`/etc/ansible/hosts`
```sh
[myLinux]

192.168.1.11 # server 2
192.168.1.12 # server 3

[myLinux:vars]

ansible_user=root
ansible_password=password
```

{: .box-error}
**Disclaimer** - Don't use root account for production

Skip the SSH key checking

`/etc/ansible/ansible.cfg`
```sh
# SSH
host_key_checking = False
```

{: .box-error}
**Disclaimer** - SSH key is needed for production

### Step 3: Check the connections

Use ping to check the connection between servers.

```sh
# Ping
ansible linux -m ping

# Reboot
ansible linux -a "reboot"
```

Done!


## Playbook - Advanced Feature
There are more and more features for you. The YAML file is used to help organize our plays (tasks).

`/etc/anisble/myTask.yml`
```yml
# Task: Check for nano
---
- name: myTask
  hosts: myLinux
  tasks:
    - name: ensure nano is there
      yum:
        name: nano
        state: latest
```

We can run the playbook task with `ansible-playbook myTask.yml`. The playbook is trying to install the latest nano. They will show you if there are any changes with the `changed=1` in the result.

Credit: Networkchuck

<br/>


