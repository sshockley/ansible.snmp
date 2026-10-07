# Ansible Role: SNMP

[![Lint](https://github.com/sshockley/ansible.snmp/actions/workflows/lint.yml/badge.svg)](https://github.com/sshockley/ansible.snmp/actions/workflows/lint.yml) [![license](https://img.shields.io/github/license/sshockley/ansible.snmp.svg?style=popout-square)](LICENSE)

## Description

Ansible role that installs and configures SNMPv3 (net-snmp) on RHEL-family and Debian/Ubuntu hosts, together with a set of [LibreNMS agent](https://github.com/sshockley/librenms-agent) extensions, and SNMP v2c on Windows.

This is a fork of [sbaerlocher.snmp](https://galaxy.ansible.com/sbaerlocher/snmp) by Simon Bärlocher.

## Installation

The role is not published on Ansible Galaxy; install it from git with a `requirements.yml`:

```yml
roles:
  - name: sshockley.snmp
    src: https://github.com/sshockley/ansible.snmp.git
    scm: git
    version: master
```

```bash
ansible-galaxy role install -r requirements.yml
ansible-galaxy collection install -r requirements.yaml   # from this repository
```

## Requirements

- Ansible >= 2.15.
- Collections listed in `requirements.yaml` (`ansible-galaxy collection install -r requirements.yaml`), including `community.general`, `ansible.windows` (>= 1.5.0), and `community.windows`.
- On RedHat-family hosts the role installs `epel-release`; the extras repository (Rocky/Alma/CentOS) must be available.
- Linux: `snmp_password` and `snmp_encryption` must be overridden (min. 8 characters).
- Windows: `snmp_community` must be set.

## Linux extensions

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

## Role Variables

| Variable                          | Default                                                                 | Comments                                                                         |
| :-------------------------------- | :---------------------------------------------------------------------- | :------------------------------------------------------------------------------- |
| snmp_user                         | snmp                                                                    | Linux: SNMPv3 user                                                               |
| snmp_password                     | snmp_password                                                           | Linux, required: SNMPv3 authentication (SHA) password                            |
| snmp_encryption                   | snmp_encryption                                                         | Linux, required: SNMPv3 privacy (AES) password                                   |
| snmp_community                    |                                                                         | Windows, required: SNMP v2c community                                            |
| snmp_permitted_managers           | []                                                                      | Windows: list of permitted SNMP managers; empty allows any host                  |
| snmp_contact                      |                                                                         | Optional: system contact                                                         |
| snmp_location                     |                                                                         | Optional: system location                                                        |
| snmp_agentaddress_protocol.ipv4/6 | udp / udp6                                                              | Optional: SNMP protocol                                                          |
| snmp_agentaddress_address.ipv4 snmp_agentaddress_address.ipv6  | ansible_default_ipv4.address ansible_default_ipv6.address | Optional: SNMP bind address ('' disables; loopback always added)              |
| snmp_agentaddress_port.ipv4 snmp_agentaddress_port.ipv6     | 161 / 161                                                               | Optional: SNMP port                                                              |
| snmp_agentx_enabled               | false                                                                   | Optional: enable AgentX                                                          |
| snmp_additional_packages          | []                                                                      | Extra packages to install with snmpd                                             |
| snmp_extension_scripts            | /usr/local/lib/snmpd                                                    | Directory for extension scripts                                                  |
| snmp_extension_list               | []                                                                      | Extra extensions: list of `url` (script) and `extend` (snmpd `extend` arguments) |
| snmp_librenms_repo                | master branch of sshockley/librenms-agent                               | Source for LibreNMS agent scripts                                                |
| snmp_include_smart                | false                                                                   | Install smartmontools and the smart-v1 extension                                 |

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
