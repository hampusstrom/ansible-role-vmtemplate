# Ansible Role: VMTemplate


An ansible role to automate the process of preparing a Linux VM guest for becoming a template in your favourite hypervisor.

## Compatibility
This role has been tested on the following platforms:
* Debian 11 Guest on Proxmox 7.3.4

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


## Example Playbook

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
