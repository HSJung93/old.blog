---
title: "Ansible Tutorial For Beginners"
date: 2022-03-30T08:33:00+09:00
categories:
  - Memo
tags:
  - ansible
# header:
#   teaser: /assets/images/code.jpg
---

## What Is Ansible
*Ansible* is a tool which can automate the configuration of all the systems. Ansible automates *configuration management* that is configuring your systems. It automates *orchestration* which means it brings together a number of applications and decides an order in which these are executed and it also automates *deployment* of the applications.

## Selecting machines from inventory
Ansible reads information about which machines you want to manage from your inventory. Although you can pass an IP address to an ad hoc command, you need inventory to take advantage of the full flexibility and repeatability of Ansible. Your inventory can store much more than IPs and FQDNs. You can create aliases, set variable values for a single host with host vars, or set variable values for multiple hosts with group vars.

> A fully qualified domain name (FQDN) is the complete domain name for a specific computer, or host, on the internet. The FQDN consists of two parts: the hostname and the domain name. For example, an FQDN for a hypothetical mail server might be mymail.somecollege.edu .

Edit(or create) `/etc/ansible/hosts` and add a few remote systems to it using either IP addresses or FQDNs:

```
192.0.2.50
aserver.example.org
bserver.example.org
```

## Connecting to remote nodes
Ansible communicates with remote machines over the SSH protocol. By default, Ansible uses native OpenSSH and connects to remote machines using your current user name, just as SSH does.

## Copying and executing modules
Once it has connected, Ansible transfers the modules required by your command or playbook to the remote machine(s) for execution.

### Action: run your first Ansible commands

Use the ping module to ping all the nodes in your inventory:
`ansible all -m ping`. You should see output for each host in your inventory, similar to this:

```
aserver.example.org | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
```

You can use `-u` as one way to specify the user to connect as, by default Ansible uses SSH, which defaults to the 'current user'. You can run a live command on all of your nodes: `ansible all -a "/bin/echo hello"`. You should see output for each host in your inventory, similar to this:

```
aserver.example.org | CHANGED | rc=0 >>
hello
```

### Action: Run your first playbook
Playbooks are used to pull together tasks into reusable units. Ansible does not store *playbooks* for you; they are simply YAML documents that you store and manage, passing them to Ansible to run as needed. In a directory of your choice you can create your first playbook in a file called mytask.yaml:

```
---
- name: My playbook
  hosts: all
  tasks:
     - name: Leaving a mark
       command: "touch /tmp/ansible_was_here"
```

You can run this command as follows: `ansible-playbook mytask.yaml`. 

### Beyond the basics

> SFTP, or Secure File Transfer Protocol, is a secure file transfer protocol that uses secure shell encryption to provide a high level of security for sending and receiving file transfers. 
> There are two primary mechanisms used for transferring the data: FTPS and SFTP. SSH FTP (SFTP) is easier to manage from a firewall point of view than SSL FTP (FTPS) based on how it encrypts, establishes, and maintains sessions

