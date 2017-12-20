# roles/resources/README.md

[![Build Status](https://travis-ci.org/cjsteel/ansible-role-resources.svg?branch=master)](https://travis-ci.org/cjsteel/ansible-role-resources)[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-resources-blue.svg)](https://galaxy.ansible.com/cjsteel/resources/)

Manage unix `skeleton`.

## Description

ansible-role-resources is an Ansible role that is used as a dependency. The role ensures for resources such as directories, clones, files, links and url downloads on the local (controller) system as well as remote systems.

## Recommendations

Running Ansible versions in a virtual environment allows for you to test with multiple Ansible version easily.

* [docs/ansible-setup](docs/ansible-setup.md)

## Requirements

* Ansible
* A dependent role that defines the resources desired.

## Variables

### group_vars

Our setup uses `project/group_vars/all/project_defaults.yml` to define project wide variable. An example is located here:

[files/group_vars/all/example_defaults.yml](files/group_vars/all/example_defaults.yml)

To install for this roles directory

```yaml
mkdir -P ../../group_vars/all
cp files/group_vars/all/example_defaults.yml ../../group_vars/all/project_defaults.yml
cat ../group_vars/all/project_defaults.yml
```

## dependent roles playbook(s)

Our setup uses multiple **set_fact** commands in our main (site.yml) playbook to set values for the variables **fact_controller_user**, **fact_controller_home** and **fact_project_path**. These variables are used by the projects `group_vars/all/project_defaults.yml` file (referred to in the above section.) The  **site.yml** file must also contain one or more includes for each role used in the project.

This example at the link below includes a single role, **csteel.myrepos**, as an example:

[files/example_site.yml](files/example_site.yml)

### The dependent roles playbook

The playbook referred to via the example **site.yml** file in our lab runs **set_fact** one more time in ordr to capture the roles directory which allows us to pass the value of the dependent roles directory to the **resources** role so that it in turn is able to find files and templates in our dependent roles directory structure.

The example here is for the role playbook for the **myrepos** role:

[files/example_myrepos.yml](files/example_myrepos.yml)

Note: When testing the **resources** role this is accomplished using the tests/vagrant.yml file for vagrant tests or the tests/travis.yml file for automated travis testing.

### The dependent roles /meta/main.yml file

Here is an example of the **dependencies** section of a dependent roles `meta/main.yml` file. In this example the dependent role is the **myrepos** role:

```shell
---
# myrepos/meta/main.yml dependencies section example

dependencies:

  - role: cjsteel.resources
    resources_on_local : '{{ myrepos_resources_on_local }}'
    resources_on_remote: '{{ myrepos_resources_on_remote }}'
```

### dependent_roles/defaults/main.yml

The contents of **myrepos_resources_on_local** and **myrepos_resources_on_remote** are defined in the dependent roles **`defaults/main.yml`** file. In the following example we define a directory structure on the local and remote system(s) using the **recursive** directive. The secondary variables **myrepos_remote_directories** and **myrepos_local_directories** are used as placeholders only and could be more descriptive in nature. You will want to define resources in the same order in which you would like them created, for example create containing directories first and so on. 

For examples on creating all of the various types of resources allowed by this role see the **tests/vagrant.yml** or **tests/travis.yml** file.

```shell
--- 
# myrepos/defaults/main.yml example

myrepos_local_user: 'deploy'
myrepos_local_resource_dir: '{{ fact_controller_home }}/.local'

myrepos_remote_user: 'deploy'
myrepos_remote_resource_dir: '/etc/skel/bin'

myrepos_resources_on_remote:

  myrepos_remote_directories:

    state          : 'directory'
    path           : '{{ myrepos_remote_resource_dir }}'
    owner          : '{{ myrepos_remote_user }}'
    group          : '{{ myrepos_remote_user }}'
    mode           : '0755'
    recursive      : True

myrepos_resources_on_local:

  myrepos_local_directories:

    state          : 'directory'
    path           : '{{ myrepos_local_resource_dir }}'
    owner          : '{{ myrepos_local_user }}'
    group          : '{{ myrepos_local_user }}'
    mode           : '0755'
    recursive      : True
```

## Testing the resources role

The resources role can be tested using the provided Vagrantfile and tests/test.yml. Run `vagrant up` in the roles directory and each task will be run in debug mode:

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

<p align="center"> 
  <img src="imgs/mcin-neuro-logo.png">
</p>

* ansible-role-resources generated using [galaxy-role-skeleton](https://github.com/cjsteel/galaxy-role-skeleton)
* This role was inspired in part by the role created in by [ALG] in 2016, [ansible-role-skel](https://github.com/AttestationLegale/ansible-role-skel)
