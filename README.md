awx
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-awx.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-awx)

Installs and optionally configures AWX for your system.

[Unit tests](https://travis-ci.org/robertdebock/ansible-role-awx) are done on every commit and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-awx/issues)

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
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/awx.png "Dependency")

Requirements
------------

- A system installed with required packages to run Ansible. Hint: [bootstrap](https://galaxy.ansible.com/robertdebock/bootstrap).
- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the last 3 release of Ansible.)
- Have Docker installed. Hint [docker](https://galaxy.ansible.com/robertdebock/bootstrap).

Role Variables
--------------

- awx_version: The version in install. [default: see `default/main.yml`]
- awx_admin_password: The initial password for the user `admin`. [default: `password`]
- awx_postgres_data_dir: The directory where to place PostgreSQL data. [default: `/var/lib/pgdata`]
- awx_postgres_database: The configuration for PostgreSQL. [default: see `default/main.yml`]
- awx_secret_key: A key used by Tower to encyrpt stuff, likely the only variable you'd like to customize. [default: `awxsecret`]
- awx_tower_verify_ssl: When speaking to Tower, can valid SSL certificates be expecte? [default: `no`]
- awx_organization: A layout describing how to configure AWX. [default: see defaults/main.yml]

Dependencies
------------

- None known.

Compatibility
-------------

This role has been tested against the following distributions and Ansible version:

|distribution|ansible 2.4|ansible 2.5|ansible 2.6|ansible 2.7|ansible devel|
|------------|-----------|-----------|-----------|-----------|-------------|
|alpine-edge*|yes|yes|yes|yes|yes*|
|alpine-latest|yes|yes|yes|yes|yes*|
|archlinux|yes|yes|yes|yes|yes*|
|centos-6|no|no|no|no|no*|
|centos-latest|yes|yes|yes|yes|yes*|
|debian-latest|yes|yes|yes|yes|yes*|
|debian-stable|yes|yes|yes|yes|yes*|
|debian-unstable*|yes|yes|yes|yes|yes*|
|fedora-latest|yes|yes|yes|yes|yes*|
|fedora-rawhide*|yes|yes|yes|yes|yes*|
|opensuse-leap|yes|yes|yes|yes|yes*|
|opensuse-tumbleweed|yes|yes|yes|yes|yes*|
|ubuntu-artful|yes|yes|yes|yes|yes*|
|ubuntu-devel*|yes|yes|yes|yes|yes*|
|ubuntu-latest|yes|yes|yes|yes|yes*|

A single star means the build may fail, it's marked as an experimental build.

Example Playbook
----------------

```
---
- name: awx
  hosts: all
  gather_facts: no
  become: yes

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.docker
    - role: robertdebock.awx
      awx_organizations:
        - name: demo
          description: Demo organization
          users:
            - name: demo
              password: demo
              email: demo@example.com
              first_name: De
              last_name: Mo
              superuser: true
          teams:
            - name: demo
              description: Demo team
          credentials:
            - name: demo_ssh
              description: demo ssh credentials
              kind: ssh
              username: demo
              password: demo
            - name: demo_scm
              description: demo scm credentials
              kind: scm
              username: Null
              password: Null
          projects:
            - name: demo
              description: demo project
              scm_credential: demo_scm
              scm_type: git
              scm_url: "https://github.com/robertdebock/ansible"
          inventories:
            - name: demo
              description: demo inventory
          job_templates:
            - name: demo
              description: demo_job_template
              project: demo
              playbook: ping.yml
              inventory: demo
              credential: demo_ssh
              job_type: run
```

To install this role:
- Install this role individually using `ansible-galaxy install robertdebock.awx`

Sample roles/requirements.yml: (install with `ansible-galaxy install -r roles/requirements.yml
```
---
- name: robertdebock.bootstrap
```

License
-------

Apache License, Version 2.0

Author Information
------------------

[Robert de Bock](https://robertdebock.nl/) <robert@meinit.nl>
