[![CircleCI](https://circleci.com/gh/ansible-roles-mamono210/vnc_server/tree/main.svg?style=svg)](https://circleci.com/gh/ansible-roles-mamono210/vnc_server/tree/main)

Role Description
=========

Installs [VNC Server](https://tigervnc.org) for CentOS Stream 9.

Requirements
------------

EPEL installed before running this role.

Role Variables
--------------

None

Dependencies
------------

None

Example Playbook
----------------

```YAML
---
- hosts: all
  become: true
  roles:
    - robertdebock.epel
    - vnc_server
```

License
-------

BSD
