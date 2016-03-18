Firewall Ansible role for Debian/Ubuntu
=======================================

[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-HanXHX.firewall-blue.svg)](https://galaxy.ansible.com/HanXHX/firewall/) [![Build Status](https://travis-ci.org/HanXHX/ansible-firewall.svg?branch=master)](https://travis-ci.org/HanXHX/ansible-firewall)

Very simple firewall for Debian/Ubuntu with UFW and fail2ban (optional).

It uses UFW default policies:

- INPUT: DROP
- OUTPUT: ACCEPT
- FORWARD : ACCEPT

In this role, you manage "INPUT" chain. FORWARD/OUTPUT will be managed in further versions.

Do NOT use this role, if you manage your own firewall!

Do NOT forget to open your SSH port if you don't use `firewall_auto_open_ssh`!

If you don't configure fail2ban, it uses default configuration.

Requirements
------------

This role uses [ufw module](http://docs.ansible.com/ansible/ufw_module.html). You need at least Ansible 1.6.

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

### About custom rule

Custom rule is a hash with:

- `proto`: any/tcp/udp/ipv6/esp/ah (default: any)
- `port`
- `policy`: allow/deny/reject (default: allow)
- `host`

### Fail2ban

- `firewall_use_fail2ban`: boolean, install and configure fail2ban (default: false)
- `firewall_fail2ban_bantime`
- `firewall_fail2ban_maxretry`
- `firewall_fail2ban_destemail`
- `firewall_fail2ban_jails`: jail list

### About Fail2ban jails

You should see `man jail.conf`

- `section`
- `enabled`
- ...

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

Only webservers (1O.0.15.0/24) and whitelisted hosts (10.255.0.12) can connect to MySQL:

    - hosts: mysql-servers
      vars:
        firewall_whitelisted_hosts:
          - '10.255.0.12'
        firewall_custom_rules:
          - proto: 'tcp'
            port: '3306'
            host: '1O.0.15.0/24'
            policy: 'allow'
      roles:
         - { role: HanXHX.firewall }


License
-------

GPLv2

Author Information
------------------

- [Twitter](https://twitter.com/hanxhx_)
- [Ansible Galaxy](https://galaxy.ansible.com/list#/users/11375)
