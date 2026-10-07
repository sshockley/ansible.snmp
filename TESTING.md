# Testing

Linux support is tested with [Molecule](https://ansible.readthedocs.io/projects/molecule/) in Docker, using systemd-enabled images.

| Scenario | Covers                                                                                                                              |
| :------- | :---------------------------------------------------------------------------------------------------------------------------------- |
| default  | Install, LibreNMS extensions, removal of stale extensions, `snmp_extension_list`, idempotence, SNMPv3 queries and rejected access |
| custom   | Custom user, loopback-only bind on a custom port, AgentX, password rotation, rejection of unsafe credentials                                |

To run locally (Linux or WSL with Docker):

```bash
python3 -m venv ~/.venvs/molecule
source ~/.venvs/molecule/bin/activate
pip install ansible-core molecule 'molecule-plugins[docker]'
ansible-galaxy collection install community.docker
ansible-galaxy collection install -r requirements.yaml
MOLECULE_DISTRO=debian12 molecule test            # default scenario
MOLECULE_DISTRO=rockylinux9 molecule test -s custom
```

With rootless Podman

```bash
python3 -m venv ~/.venvs/molecule
source ~/.venvs/molecule/bin/activate
pip install ansible-core molecule 'molecule-plugins[podman]'
ansible-galaxy collection install containers.podman
ansible-galaxy collection install -r requirements.yaml
MOLECULE_DISTRO=debian12 molecule -c .config/molecule/podman.yml test -s custom
```

`MOLECULE_DISTRO` selects a `geerlingguy/docker-<distro>-ansible` image. Windows is not covered by the tests.
