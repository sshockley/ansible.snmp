# Role Variables

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
| snmp_agentaddress_address.ipv4 snmp_agentaddress_address.ipv6  | ansible_facts['default_ipv4']['address'] ansible_facts['default_ipv6']['address'] | Optional: SNMP bind address ('' disables; loopback always added)              |
| snmp_agentaddress_port.ipv4 snmp_agentaddress_port.ipv6     | 161 / 161                                                               | Optional: SNMP port                                                              |
| snmp_agentx_enabled               | false                                                                   | Optional: enable AgentX                                                          |
| snmp_additional_packages          | []                                                                      | Extra packages to install with snmpd                                             |
| snmp_extension_scripts            | /usr/local/lib/snmpd                                                    | Directory for extension scripts                                                  |
| snmp_extension_list               | []                                                                      | Extra extensions: list of `url` (script) and `extend` (snmpd `extend` arguments); scripts dropped from the list are removed |
| snmp_librenms_repo                | master branch of sshockley/librenms-agent                               | Source for LibreNMS agent scripts                                                |
| snmp_include_smart                | false                                                                   | Install smartmontools and the smart-v1 extension                                 |
| snmp_cron_osupdates               | 0 * * * *                                                               | cron schedule for refreshing the osupdates extension's data                      |
| snmp_cron_systemd                 | 0 * * * *                                                               | cron schedule for refreshing the systemd extension's data                        |
| snmp_cron_smart                   | 0 * * * *                                                               | cron schedule for refreshing the smart-v1 extension's cache                      |
