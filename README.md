Ansible Role - lmod
===================

This role installs Lmod(Lua based Environment Modules).

Platforms
---------

CentOS 6.7 is the only platform that is supported and tested so far.

Dependencies
------------

None.

Installation
------------

Regular installation:

```
ansible-galaxy install RDI2.lmod
```

Installation to a specific directory(e.g. roles/):

```
ansible-galaxy install -p roles/ RDI2.lmod
```


Example Playbook
----------------

Include the role like this:

```yaml
- hosts: servers
  roles:
     - { role: RDI2.lmod }
```

License
-------

MIT License.

Author
------

- Koji Tanaka - RDI2
