Wireguard
=========

Installs VPN with WireGuard for multiple instances.

[Wireguard Configuration File Format ](https://wiresock.net/documentation/wireguard/config.html)

Requirements
------------

No requirements.


Role Variables
--------------

Required:
```
wireguard_address: 10.0.0.2/32, fd86:ea04:1115::1/128 # IP address in VPN
wireguard_endpoint: vpn.example.com:51820 # IP address in insecure framework
```


Dependencies
------------

No dependencies.


Example Playbook
----------------

#### Install WireGuard and create keys

Installing WireGuard and creating the keys is a single time process.
```
- name: Install WireGuard and create keys, should be made only once
  hosts: wireguard
  gather_facts: yes

  tasks:
    - name: Install WireGuard
      import_role:
        name: wireguard
        tasks_from: install-wireguard.yml

    - name: Create keys
      import_role:
        name: wireguard
        tasks_from: create-keys.yml
```

#### Configure WireGuard

Configuring WireGuard must be made in two steps/playbooks:
1. read keys for all hosts and storing them in hostvars.
2. configuring WireGuard reading the keys for all hosts from hostvars.
```
- name: Read keys and storing them in hostvars
  hosts: wireguard
  gather_facts: yes

  tasks:
  # must be invoked for all hosts in the `wireguard` group
  # for propagating keys in hostvars.
    - name: Read keys
      import_role:
        name: wireguard
        tasks_from: read-keys.yml



- name: Configure WireGuard using the keys from hostvars
  hosts: wireguard
  gather_facts: no

  tasks:
  # must be invoked for all hosts in the `wireguard` group
  # otherwise not all peers are stored in the WireGuard configuration.
    - name: Configure wireguard
      import_role:
        name: wireguard
        tasks_from: configure-wireguard.yml

```

License
-------

MIT

Author Information
------------------

© 2025 Pawel Idzikowski
