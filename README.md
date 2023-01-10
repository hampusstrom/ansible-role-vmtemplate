# Role Name


An ansible role to automate the process of preparing a Linux VM guest for becoming a template in your favourite hypervisor.

## Installation
### ansible-galaxy
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

Requires root: yes

A virtual machine running on either:
* XCP-NG
* XenServer
* Proxmox

### XCP-NG
Requires the guest tools ISO to be mounted as a cdrom device on the guest.
The role will automatically mount /dev/sr0 (can be changed using vmtemplate_cdrom_path) and install the guest tools from there.

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

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
