Firewall Ansible role for Debian/Ubuntu
=======================================

Simple firewall for Debian/Ubuntu with UFW.

Requirements
------------

This role uses [ufw module](http://docs.ansible.com/ansible/ufw_module.html). You need at least Ansible 1.6.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

None.

Example Playbook
----------------

### Simple webserver

    - hosts: web-servers
      vars:
        firewall_open_tcp_ports: [22, 80, 443]
      roles:
         - { role: HanXHX.firewall }

### Simple MySQL/MariaDB server

Only webservers and whitelisted hosts can connect to MySQL:

    - hosts: mysql-servers
      vars:
        firewall_open_tcp_ports: [22]
        firewall_whitelisted_hosts:
          - '10.255.0.12'
        firewall_custom_rules:
          - proto: 'tcp'
            port: '3306'
            host: '1O.0.15.0/24'
      roles:
         - { role: HanXHX.firewall }


License
-------

GPLv2

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
