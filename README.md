Firewall Ansible role for Debian/Ubuntu
=======================================

[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-HanXHX.firewall-blue.svg)](https://galaxy.ansible.com/HanXHX/firewall/) [![Build Status](https://app.travis-ci.com/HanXHX/ansible-firewall.svg?branch=master)](https://app.travis-ci.com/HanXHX/ansible-firewall)

Very simple firewall for Debian/Ubuntu with UFW.

It uses UFW default policies:

- INPUT: DROP
- OUTPUT: ACCEPT
- FORWARD : ACCEPT

In this role, you manage "INPUT" chain. FORWARD/OUTPUT will be managed in further versions.

Do NOT use this role, if you manage your own firewall!

Do NOT forget to open your SSH port if you don't use `firewall_auto_open_ssh`!

Requirements
------------

- Ansible >= 2.11
- Collections: community.general / ansible.netcommon

Role Variables
--------------

### Common

- `firewall_open_tcp_ports`: Input TCP open ports list
- `firewall_open_udp_ports`: Input UDP open ports list

### UFW Configuration

- `firewall_ipv6`: Enable/disable IPv6 support (default is true)
- `firewall_reset`: Reset all rules (it breaks idempotence!). Usefull when you want to clean and recreate all rules.
- `firewall_logging`: iptables loglevel (values: on/off/low/medium/high/full, default is low)
- `firewall_modules`: kernel modules list (useful when you need NAT+FTP). For now, you don't need to add modules (default is empty list)

### Firewall

- `firewall_auto_open_ssh`: auto open current SSH port (default: true)
- `firewall_whitelisted_hosts`: whitelisted hosts (IP) list
- `firewall_blacklisted_hosts`: backlisted hosts (IP) list
- `firewall_custom_rules`: custom rule list (see bellow)

### DNS

- `firewall_whitelisted_dns`: whitelisted hosts (IPv4 & IPv6) list
- `firewall_blacklisted_dns`: backlisted hosts (IPv4 & IPv6) list

Please note, DNS requests is done before insert UFW rules. You **must not** use this feature with a Dyn-DNS solution.

### About custom rule

Custom rule is a hash. Check [UFW module doc](ihttp://docs.ansible.com/ansible/ufw_module.html). Please note routed feature is available with UFW 0.34+ (Stretch).

Dependencies
------------

None.

Example Playbook
----------------

### Simple webserver

    - hosts: web-servers
      vars:
        firewall_open_tcp_ports: [80, 443]
      roles:
         - { role: HanXHX.firewall }

### Simple MySQL/MariaDB server

Only webservers (10.0.15.0/24) and whitelisted hosts (10.255.0.12) can connect to MySQL:

    - hosts: mysql-servers
      vars:
        firewall_whitelisted_hosts:
          - '10.255.0.12'
        firewall_custom_rules:
          - proto: 'tcp'
            port: '3306'
            host: '10.0.15.0/24'
            policy: 'allow'
      roles:
         - { role: HanXHX.firewall }


License
-------

GPLv2

Donation
--------

If this code helped you, or if you’ve used them for your projects, feel free to buy me some :beers:

- Bitcoin: `1BQwhBeszzWbUTyK4aUyq3SRg7rBSHcEQn`
- Ethereum: `63abe6b2648fd892816d87a31e3d9d4365a737b5`
- Litecoin: `LeNDw34zQLX84VvhCGADNvHMEgb5QyFXyD`
- Monero: `45wbf7VdQAZS5EWUrPhen7Wo4hy7Pa7c7ZBdaWQSRowtd3CZ5vpVw5nTPphTuqVQrnYZC72FXDYyfP31uJmfSQ6qRXFy3bQ`

No crypto-currency? :star: the project is also a way of saying thank you! :sunglasses:

Author Information
------------------

- [Twitter](https://twitter.com/hanxhx_)
- [Ansible Galaxy](https://galaxy.ansible.com/HanXHX/)
