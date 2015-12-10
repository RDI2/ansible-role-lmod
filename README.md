Ansible Role - lmod
===================

This role installs Lmod(Lua based Environment Modules).

Platforms
---------

CentOS 6.7 is the only platform that is supported and tested so far.

Dependencies
------------

None.

Example Playbook
----------------

Include the role like this:

```yaml
- hosts: servers
  roles:
     - { role: rdi2sys.lmod }
```

License
-------

MIT License.

Author
------

- Koji Tanaka - RDI2
