reboot
======

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-reboot.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-reboot

The purpose of this role is to reboot your system.

[Unit tests](https://travis-ci.org/robertdebock/ansible-role-reboot are done on every commit and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-reboot/issues)

To test this role locally please use [Molecule](https://github.com/metacloud/molecule):
```
pip install molecule
molecule test
```
There are many scenarios available, please have a look in the `molecule/` directory.

Context
--------
This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/rebootpng "Dependency")

Requirements
------------

- A system installed with required packages to run Ansible. Hint: [bootstrap](https://galaxy.ansible.com/robertdebock/bootstrap).
- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the last 3 release of Ansible.)

Role Variables
--------------

reboot_delay: How long to wait in seconds before sending a reboot. [default: 4]
reboot_up_delay: Number of seconds to wait before checking if the machine is up. [default: 8]
reboot_message: Include a personalized message that will be stored in logs. [default: see `defaults/main.yml`]

Dependencies
------------

- None known.

Compatibility
-------------

This role has been tested against the following distributions and Ansible version:

|distribution|ansible 2.4|ansible 2.5|ansible 2.6|ansible-2.7|ansible-devel|
|------------|-----------|-----------|-----------|-----------|-------------|
|alpine-edge*|yes|yes|yes|yes|yes*|
|alpine-latest|yes|yes|yes|yes|yes*|
|archlinux|yes|yes|yes|yes|yes*|
|centos-6|yes|yes|yes|yes|yes*|
|centos-latest|yes|yes|yes|yes|yes*|
|debian-latest|yes|yes|yes|yes|yes*|
|debian-stable|yes|yes|yes|yes|yes*|
|debian-unstable*|yes|yes|yes|yes|yes*|
|fedora-latest|yes|yes|yes|yes|yes*|
|fedora-rawhide*|yes|yes|yes|yes|yes*|
|gentoo|yes|yes|yes|yes|yes*|
|opensuse-leap|yes|yes|yes|yes|yes*|
|opensuse-tumbleweed|yes|yes|yes|yes|yes*|
|ubuntu-artful|yes|yes|yes|yes|yes*|
|ubuntu-devel*|yes|yes|yes|yes|yes*|
|ubuntu-latest|yes|yes|yes|yes|yes*|

The star means the build may fail, it's marked as an experimental build.

Example Playbook
----------------

```
---
- name: reboot
  hosts: all
  gather_facts: no
  become: yes

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.reboot
```

You can also include a role when something changed.

```
- name: reboot
  hosts: all
  gather_facts: no
  become: yes

  tasks:
    - name: do something that requires a reboot.
      sestatus:
        state: disabled
      register do_something

    - name: include reboot role
      include_role:
        name: robertdebock.reboot
      when:
        - do_something.changed
```

Normally a `notify` using handlers is perfect for changed tasks, but the module `include_role` [can't be used in a handler](https://github.com/ansible/ansible/issues/35542).

Linting will suggest to move that `include_role` to a handler. To instruct ansible-lint to ignore this issue, use one of these two methods:

1. Tag the task
In your `playbook.yml`:
```
    - name: include reboot role
      include_role:
        name: robertdebock.reboot
      when:
        - do_something.changed
      tags:
        - skip_ansible_lint
```

2. Let molecule skip a test
In molecule/*/molecule.yml:
```
provisioner:
  name: ansible
  lint:
    name: ansible-lint
    options:
      x:
        - ANSIBLE0016
```

To install this role:
- Install this role individually using `ansible-galaxy install robertdebock.reboot

Sample roles/requirements.yml: (install with `ansible-galaxy install -r roles/requirements.yml
```
---
- name: robertdebock.bootstrap
- name: robertdebock.reboot
```

License
-------

Apache License, Version 2.0

Author Information
------------------

[Robert de Bock](https://robertdebock.nl/) <robert@meinit.nl>
