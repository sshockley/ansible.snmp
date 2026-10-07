# Ansible Role: SNMP

[![Lint](https://github.com/sshockley/ansible.snmp/actions/workflows/lint.yml/badge.svg)](https://github.com/sshockley/ansible.snmp/actions/workflows/lint.yml) [![license](https://img.shields.io/github/license/sshockley/ansible.snmp.svg?style=popout-square)](LICENSE)

## Description

Ansible role that installs and configures SNMPv3 (net-snmp) on RHEL-family and Debian/Ubuntu hosts, together with a set of [LibreNMS agent](https://github.com/sshockley/librenms-agent) extensions, and SNMP v2c on Windows.

This is a fork of [sbaerlocher.snmp](https://galaxy.ansible.com/sbaerlocher/snmp) by Simon Bärlocher.

## Installation

The role is not published on Ansible Galaxy. Install it from git, together with the collections it needs, using a `requirements.yml` in your project:

```yml
roles:
  - name: sshockley.snmp
    src: https://github.com/sshockley/ansible.snmp.git
    scm: git
    version: master

# Same versions as collections/requirements.yml
collections:
  - name: ansible.windows
    version: ">=1.5.0,<4.0.0"
  - name: community.general
    version: ">=13.0.0,<14.0.0"
  - name: community.windows
    version: ">=3.0.0,<4.0.0"
```

```bash
ansible-galaxy install -r requirements.yml
```

## Requirements

- Ansible >= 2.15.
- The collections in [`collections/requirements.yml`](collections/requirements.yml): `ansible.windows`, `community.general` and `community.windows`.
- On RedHat-family hosts the role installs `epel-release`; the extras repository (Rocky/Alma/CentOS) must be available.
- Linux: `snmp_password` and `snmp_encryption` must be overridden (min. 8 characters).
- Windows: `snmp_community` must be set.

## LibreNMS extensions

The role also installs some LibreNMS extensions from `snmp_librenms_repo`. When the condition stops applying the extension is removed.

| Extension          | Installed when                                        |
| :----------------- | :---------------------------------------------------- |
| distro             | always                                                |
| osupdates          | always                                                |
| systemd            | systemd is in use                                     |
| linux_config_files | EL-family hosts                                       |
| zfs                | The `zfs` or `zfs-zed` package is installed           |
| proxmox            | Proxmox (`pve`) kernel                                |
| smart-v1           | `snmp_include_smart` is true and the host is not a VM |


## Role variables
See [VARIABLES.md](VARIABLES.md)

## Dependencies

None

## Example Playbook

```yml
- hosts: all
  roles:
    - role: sshockley.snmp
      vars:
        snmp_password: "{{ vault_snmp_password }}"
        snmp_encryption: "{{ vault_snmp_encryption }}"
        snmp_location: "Server room"
        snmp_contact: "ops@example.com"
```

## Authors

- Steve Shockley (maintainer of this fork)
- [Simon Bärlocher](https://sbaerlocher.ch) (original author)

## License

This project is under the MIT License. See the [LICENSE](LICENSE) file for the full license text.

## Copyright

(c) 2018, Simon Bärlocher
