# [Ansible role gitlab_runner_docker](#gitlab_runner_docker)

Install and configures GitLab Runner in a docker container

|GitHub|Downloads|Version|
|------|---------|-------|
|[![github](https://github.com/mullholland/ansible-role-gitlab_runner_docker/actions/workflows/molecule.yml/badge.svg)](https://github.com/mullholland/ansible-role-gitlab_runner_docker/actions/workflows/molecule.yml)|[![downloads](https://img.shields.io/ansible/role/d/mullholland/gitlab_runner_docker)](https://galaxy.ansible.com/mullholland/gitlab_runner_docker)|[![Version](https://img.shields.io/github/release/mullholland/ansible-role-gitlab_runner_docker.svg)](https://github.com/mullholland/ansible-role-gitlab_runner_docker/releases/)|
## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/mullholland/ansible-role-gitlab_runner_docker/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true
  vars:
    gitlab_runner_docker_instances:
      - name: "group"
        image: "registry.gitlab.com/gitlab-org/gitlab-runner:alpine"
        server: "https:/gitlab.example.com"
        token: "glrt-GnhtWxnfAxn3GnT4ExiK"
        env:
          - "KEY1: VALUE1"
        register_env:
          - "KEY2: VALUE2"
        # command: ""
        volumes:
          - "/tmp:/tmp"
        ports:
          - "8080:8080"
        labels:
          - "Label1=Tag1"
        state: present
      - name: "group2"
        image: "registry.gitlab.com/gitlab-org/gitlab-runner:alpine"
        server: "https:/gitlab.example.com"
        token: "glrt-GnhtWxnfAxn3GnT4ExiK"
        env:
          - "KEY1: VALUE1"
        register_env:
          - "KEY2: VALUE2"
        # command: ""
        volumes:
          - "/tmp:/tmp"
        ports:
          - "8080:8080"
        labels:
          - "Label1=Tag1"
        state: present
      - name: "group3"
        state: absent

  roles:
    - role: "mullholland.gitlab_runner_docker"
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/mullholland/ansible-role-gitlab_runner_docker/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: true
  gather_facts: true
  vars:
    pip_packages:
      - "docker"

  roles:
    - role: mullholland.docker
    - role: mullholland.repository_epel
    - role: mullholland.pip
```



## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/mullholland/ansible-role-gitlab_runner_docker/blob/master/defaults/main.yml):

```yaml
---
# General config
gitlab_runner_docker_network_name: "gitlab"
gitlab_runner_docker_base_path: "/opt"
gitlab_runner_docker_timezone: "Europe/Berlin"

# User/Group of the stack. Everything is mapped to this, instead of root.
gitlab_runner_docker_user: "homelab"
gitlab_runner_docker_uid: "900"
gitlab_runner_docker_group: "homelab"
gitlab_runner_docker_gid: "900"
gitlab_runner_docker_user_system: true

# options
# https://docs.gitlab.com/runner/commands/index.html#using-environment-variables
# gitlab-runner install --help
gitlab_runner_docker_registration_envs:
  - "REGISTER_LOCKED: true"
  - "RUNNER_EXECUTOR: docker"
  - "DOCKER_IMAGE: docker"
  - "DOCKER_VOLUMES: '/var/run/docker.sock:/var/run/docker.sock'"

gitlab_runner_docker_instances:
  - name: ""          # (required). Name of the GitLab runner instance
    image: ""         # (optional). Which image to use. Defaults to "registry.gitlab.com/gitlab-org/gitlab-runner:alpine".
    server: ""        # (required). GitLab serer URL
    token: ""         # (required). GitLab registration token.
    env: []           # (optional). Add additional environment variables only for gitlab runner.
    register_env: []  # (optional). Add additional environment variables only for runner registration. see option with 'gitlab-runner install --help'
    register_cmd: []  # (optional). Add additional commandline options for runner registration
    volumes: []       # (optional). Add additional volume mappings.
    ports: []         # (optional). Map docker port to the Host, can be used to exposed metrics or similar.
    labels: []        # (optional). Add container labels.
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/mullholland/ansible-role-gitlab_runner_docker/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[mullholland.repository_epel](https://galaxy.ansible.com/mullholland/repository_epel)|[![Build Status GitHub](https://github.com/mullholland/ansible-role-repository_epel/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-repository_epel/actions)|[![Build Status GitLab](https://gitlab.com/opensourceunicorn/ansible-role-repository_epel/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-repository_epel)|
|[mullholland.docker](https://galaxy.ansible.com/mullholland/docker)|[![Build Status GitHub](https://github.com/mullholland/ansible-role-docker/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-docker/actions)|[![Build Status GitLab](https://gitlab.com/opensourceunicorn/ansible-role-docker/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-docker)|
|[mullholland.pip](https://galaxy.ansible.com/mullholland/pip)|[![Build Status GitHub](https://github.com/mullholland/ansible-role-pip/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-pip/actions)|[![Build Status GitLab](https://gitlab.com/opensourceunicorn/ansible-role-pip/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-pip)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://mullholland.net) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/mullholland/ansible-role-gitlab_runner_docker/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/mullholland):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/r/mullholland/enterpriselinux)|all|
|[Fedora](https://hub.docker.com/r/mullholland/fedora/)|38, 39|
|[Ubuntu](https://hub.docker.com/r/mullholland/ubuntu)|all|
|[Debian](https://hub.docker.com/r/mullholland/debian)|all|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/mullholland/ansible-role-gitlab_runner_docker/issues).

## [License](#license)

[MIT](https://github.com/mullholland/ansible-role-gitlab_runner_docker/blob/master/LICENSE).

## [Author Information](#author-information)

[mullholland](https://mullholland.net)
