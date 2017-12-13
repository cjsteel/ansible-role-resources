# roles/resources/README.md

[![Build Status](https://travis-ci.org/cjsteel/resources.svg?branch=master)](https://travis-ci.org/cjsteel/resources)
[![Travis CI](http://img.shields.io/travis/csteel/resources/default.svg?style=flat)](http://travis-ci.org/csteel/resources/default)
[![Platforms](http://img.shields.io/badge/platforms-debian%20/%20ubuntu-lightgrey.svg?style=flat)](#)

## Description

ansoble-role-resources is an Ansible role that is used as a dependency. The role ensures for resources such as directories, files, links and downloads on local (controller) and remote systems.

## Recommended

Running Ansible in a virtual environment allows for a lot of testing flexibility.

* [docs/ansible-setup](docs/ansible-setup.md)

## Requirements

* Ansible
* ​

## Variables

### project/group_vars/all/project_defaults.yml

Example: [files/group_vars/all/example_defaults.yml](files/group_vars/all/example_defaults.yml)

To install for this roles directory

```yaml
mkdir -P ../../group_vars/all
cp files/group_vars/all/example_defaults.yml ../../group_vars/all/project_defaults.yml
cat ../group_vars/all/project_defaults.yml
```

### dependent_role/meta/main.yml

Example of `dependent_role/meta/main.yml`

```shell
dependencies:

  - role: resources
    resources_on_local : '{{ dependent_role_resources_on_local }}'
    resources_on_remote: '{{ dependent_role_resources_on_remote }}'
```

### dependent_role/defaults/main.yml

The resources defined in the dependent roles `defaults/main.yml` are created by adding the containing variables, `dependent_role_resources_on_remote` and `dependent_role_resources_on_remote` in this example, to the dependent roles `meta/main.yml` file as illustrated in the above example.

```shell
---
## dependent_role/defaults/main.yml
#
#
dependent_role_remote_user: 'deploy'
dependent_role_remote_resource_dir: '/etc/skel/bin'

dependent_role_local_user: 'deploy'
dependent_role_local_resource_dir: '{{ fact_controller_home }}/sw/example'

dependent_role_resources_on_remote:

  dependent_role_remote_directories:

    state          : 'directory'
    path           : '{{ dependent_role_remote_resource_dir }}'
    owner          : '{{ dependent_role_remote_user }}'
    group          : '{{ dependent_role_remote_user }}'
    mode           : '0755'
    recursive      : True

dependent_role_resources_on_local:

  dependent_role_local_directories:

    state          : 'directory'
    path           : '{{ dependent_role_local_resource_dir }}'
    owner          : '{{ dependent_role_local_user }}'
    group          : '{{ dependent_role_local_user }}'
    mode           : '0755'
    recursive      : True
```

## Testing the resources role

Move into the roles directory and run `vagrant up`:

```shell
cd roles/resources
vagrant up
```

## Authors and License

- [Christopher Steel](http://mcin-cnim.ca/) | [e-mail](mailto:christopher.steel@mcgill.ca)
- [John Le](http://mcin-cnim.ca/) | [e-mail](mailto:john.le@mcgill.ca)
- [Andy Teng](http://mcin-cnim.ca/) | [e-mail](xiaoqiu.teng@mcgill.ca)

License: [MIT](https://tldrlegal.com/license/mit-license)

***
## Open Science

The Neuro has adopted the principles of Open Science. We are inspired by the likes of the Allen Institute for Brain Science, the National Institutes of Health's Human Connectome project, and the Human Genome project. For additional information, please see [Open Science at the Neuro](https://www.mcgill.ca/neuro/open-science-0).

![neuro](imgs/mcin-neuro-logo.png)



* ansible-role-resources generated using [galaxy-role-skeleton](https://github.com/cjsteel/galaxy-role-skeleton)
