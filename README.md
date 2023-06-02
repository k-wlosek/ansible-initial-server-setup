# Ansible playbook for initial server setup on Ubuntu

This playbook is used to setup a new Ubuntu server with some basic tools and
configuration. It is intended to be used as a starting point for new servers.

## Features
The playbook: updates the packages, installs packages defined in `essential_packages`,
creates a new user, creates a ssh key for the new user, hardens SSH, installs
and configures fail2ban, installs and configures UFW, installs and configures
unattended-upgrades.

## Usage
Create a `hosts` file and define the servers you want to setup.
```ini
[group_name]
name ansible_host=ip_address ansible_user=root ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_connection=ssh ansible_ssh_port="{{ ssh_port }}"
```

Create a `vars.yml` file in `group_vars/group_name/` and define the variables or define them in Ansible Vault.
```yaml
---
username: user
password: changeme

ssh_port: 22
ssh_root_login: "no"
incident_notify_email: ""
fail2ban_bantime: "1h"
fail2ban_findtime: "3m"
fail2ban_maxretry: 5
open_ports:
  - 80
  - 443

essential_packages:
  - vim
  - git
  - curl
  - htop

```
Other variables can be found in `roles/system/defaults/main.yml`.

Run the playbook.
```bash
ansible-playbook --ask-vault-pass playbook.yml 
```
