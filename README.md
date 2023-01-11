# Ansible Role: VMTemplate


An ansible role to automate the process of preparing a Linux VM guest for becoming a template in your favourite hypervisor.

This role is designed to be run AFTER you've made all your own customizations.
Installing packages, running hardening baselines etc.

Steps taken:
* Set the timezone
* Install guest tools for the appropriate hypervisor
* Set a generic hostname
* Reboot the host to ensure no rogue processes/file locks exist for the user we're about to delete
* Delete the current user by allowing root ssh login temporarily
* Delete the home directory of the current user
* Set the root user password, generate a strong password if not defined
* Re-disable root ssh login
* Update all packages
* Delete SSH host keys (New ones are generated on first boot)
* Delete log files
* Delete root user .bash_history
* Clear /etc/machine-id and /usr/lib/dbus/machine-id
* Delete root user ssh keys
* Install cloud-init and ensure it starts on boot
* Shutdown the host when finished to not get dirty with log files again.


## Compatibility
This role has been tested on the following platforms:
* Debian 11 Guest on Proxmox 7.3.4
* Debian 11 Guest on XCP-ng 8.2.1

## Installation
### ansible-galaxy **Soon(tm)**
```
NotImplementedException on line 1: ansible-galaxy install hampusstrom.vmtemplate
```

### Manual Installation
Current user only:
```
git clone https://github.com/hampusstrom/ansible-role-vmtemplate.git ~/.ansible/roles/hampusstrom.vmtemplate
```
System wide:
```
git clone https://github.com/hampusstrom/ansible-role-vmtemplate.git /etc/ansible/roles/hampusstrom.vmtemplate
```


## Requirements

### Init System(s): **systemd**
### Requires root: **yes**

A linux virtual machine guest running on either:
* XCP-ng
* XenServer
* Proxmox
* KVM

**`ansible_user` MUST BE DEFINED**
Either:

Add ansible_user=username to your inventory
```ini
[mytemplatevm]
mycoolvm ansible_host=192.168.1.2 ansible_user=username
```

Or run the playbook with `-u username`

`ansible-playbook -i 'inventory' -u username -K example-playbook.yml`



Since we require root, use this role in a playbook that has `become:yes` globally defined or call this role using the `become: yes` keyword.


```yaml
- hosts: mytemplatevm
  become: yes
  roles:
    - role: hampusstrom.vmtemplate

# OR

- hosts: mytemplatevm
  roles:
    - role: hampusstrom.vmtemplate
      become: yes
```

### XCP-ng/XenServer
Requires the XE guest tools ISO to be mounted as a cdrom device on the guest.

The role will automatically mount /dev/sr0 (can be changed using `vmtemplate_cdrom_path`) and install the guest tools from there.

## Role Variables

### vmtemplate_cdrom_path
The path to the cdrom drive from which to mount the XCP-NG guest iso.

default: `/dev/sr0`

### vmtemplate_cdrom_mount_path
The path to mount the XCP-ng guest tools iso to.

default: `/mnt`

### vmtemplate_timezone
The timezone to configure the hosts clock for.

default: `Europe/Stockholm`

### vmtemplate_root_pass
The password to set for the root user.

If not provided, a 32 character password will be generated for you.

**This password will not be shown to you. Use sudo with your cloud-init created users.**

default: `randomly generated 32 character password.`

## Example Playbook

Run using:
`ansible-playbook -i "inventory" -u username -K example-playbook.yml`

```yaml
---
- hosts: mytemplatevm
  gather_facts: true
  become: true
  roles:
    - hampusstrom.vmtemplate
```
License
-------

MIT
