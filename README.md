docker-generic-image
============

[![Build Status](https://travis-ci.org/wangsha/docker-generic-image.svg?branch=master)](https://travis-ci.org/wangsha/docker-generic-image)
[![Ansible Galaxy](https://img.shields.io/badge/AnsibleGalaxy-wangsha.docker--generic--image-blue.svg)](https://galaxy.ansible.com/wangsha/docker-generic-image/)

Ansible role to manage and run a docker container from any given image.

Requirements
------------

This role has only been tested on Ubuntu 14.04. Since this uses Ansible's
docker module, you will need to ensure that a recent-ish version of `docker-py`
and `docker` are installed.

Examples
--------

Install this module from Ansible Galaxy into the './roles' directory:
```bash
ansible-galaxy install wangsha.docker-generic-image -p ./roles
```

Use it in a playbook as follows, assuming you already have docker setup:
```yaml
- hosts: 'servers'
  roles:
    - role: angstwad.docker_ubuntu
      become: true
    - role: wangsha.docker-generic-image
      become: true
      docker_container_name: hello-world
      docker_container_image: hello-world
```

Have a look at the [defaults/main.yml](defaults/main.yml) for role variables
that can be overridden.


If you need a playbook to set Docker itself, have a look at [angstwad.docker_ubuntu](https://github.com/angstwad/docker.ubuntu) Galaxy
role.


Custom volume mappings
----------------------
Docker allows mounting a host directory or a host file as [data volume](https://docs.docker.com/engine/userguide/containers/dockervolumes/).
This role mounts host directories to persist container data and host files to configure container behavior.
`docker_generic-image_directory_volumes` and `docker_generic-image_file_volumes` are the two variables to control volume mappings.
If you wish to customize the mapping, please follow `<host directory>:<container directory>:<mapping mode>` format
 to ensure host directories are correctly created before launching containers.
 
To customize host file mappings, update `docker_generic-image_file_volumes`. 
This role will automatically create file parent directories and copy the template 
to host machine. The naming convention for template is `<host_file_name>.<host_file_extension>.j2`.
To copy template from your own ansible diretories, set `docker_generic-image_template_path`.

Example Config:
```yaml
docker_container_file_volumes:
  - '/opt/myapp/conf/settings.conf:/etc/myapp/conf/settings.conf:ro'
docker_container_template_path: /path/to/ansible/project/templates/
# make sure file /path/to/ansible/project/templates//settings.conf.j2 exists. 
```

Additional References
---------------------
- [docker cadvisor role](https://galaxy.ansible.com/wangsha/docker-cadvisor/)
- [docker redis role](https://galaxy.ansible.com/wangsha/docker-redis/)

License
-------

[MIT](LICENSE.txt)

Author Information
------------------

- wangsha
