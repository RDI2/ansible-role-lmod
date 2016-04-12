# Ansible/role/tests with Docker

This is a simple handy template of Ansible `<role>/tests` that lets you test a role with
Docker container. It uses `make` command on top, but does not require any other additional
component.

## Supported container distros and versions

| Distro | Versions          |
|:-------|:------------------|
|Ubuntu  |14.04, 15.10, 16.04|
|Debian  |Wheezy, Jessie     |
|Fedora  |22, 23             |
|CentOS  |6, 7               |
|OpenSUSE|13.2               |

> **Note:** You may want to take a look at the Dockerfiles in `Dockerfile.d`.
> Each of them pulls an image from the official repository of each distro and
> enable ssh key authentication.

## Installation

Create a role with `ansible-galaxy` and replace the `tests` directory like this:

```bash
$ ansible-galaxy init new-playbook
$ cd new-playbook
$ rm -rf tests
$ git clone https://github.com/kjtanaka/ansible-role-tests-with-docker.git tests
$ cd tests
```

## How to use

Take a look at `Makefile`, and update variables as you need. The default variables are
set for the default environment of the Docker Toolbox on Mac OS X.

```bash
# Initialize the environment
$ make init

# Test your playbook
$ make provision

# Log into the container
$ make ssh

# Destroy container and provision it from scratch
$ make reprovision

# Clean it
$ make clean
```

Basically, everytime you update your role, you just need to execute `make provision`,
and then when you want to test the role from scratch, execute `make reprovision`.
After you finish everything, then clean it by `make clean`.

## License

MIT license.

## Acknowledgements

The idea is inspired by these things:

- [Vagrant](https://www.vagrantup.com/)
- [tutumcloud/tutum-centos](https://github.com/tutumcloud/tutum-centos.git)
- [Dockerizing an SSH daemon service](https://docs.docker.com/engine/examples/running_ssh_service/)
