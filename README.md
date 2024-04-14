# Ansible role logindefs

![GitHub](https://img.shields.io/github/license/jomrr/ansible-role-logindefs) ![GitHub last commit](https://img.shields.io/github/last-commit/jomrr/ansible-role-logindefs) ![GitHub issues](https://img.shields.io/github/issues-raw/jomrr/ansible-role-logindefs)

**Ansible role for configuring /etc/login.defs.**

## Description

This Ansible role installs and configures logindefs on supported platforms.

## Prerequisites

This role has no special prerequisites.

### System packages (Fedora)

- `python3` (Python 3.8 or later)

### Python (requirements.txt)

- ansible >= 2.15

## Dependencies (requirements.yml)

This role has no dependencies.

## Supported Platforms

| OS Family | Distribution | Version | Container Image |
|-----------|--------------|---------|-----------------|
| RedHat | AlmaLinux | 8 | [jomrr/molecule-almalinux:8]( https://hub.docker.com/r/jomrr/molecule-almalinux ) |
| | | 9 | [jomrr/molecule-almalinux:9]( https://hub.docker.com/r/jomrr/molecule-almalinux ) |
| Alpine | Alpine | 3.18 | [jomrr/molecule-alpine:3.18]( https://hub.docker.com/r/jomrr/molecule-alpine ) |
| | | 3.19 | [jomrr/molecule-alpine:3.19]( https://hub.docker.com/r/jomrr/molecule-alpine ) |
| Archlinux | Archlinux | latest | [jomrr/molecule-archlinux:latest]( https://hub.docker.com/r/jomrr/molecule-archlinux ) |
| Debian | Debian | 11 | [jomrr/molecule-debian:11]( https://hub.docker.com/r/jomrr/molecule-debian ) |
| | | 12 | [jomrr/molecule-debian:12]( https://hub.docker.com/r/jomrr/molecule-debian ) |
| | | 13 | [jomrr/molecule-debian:13]( https://hub.docker.com/r/jomrr/molecule-debian ) |
| RedHat | Fedora | 39 | [jomrr/molecule-fedora:39]( https://hub.docker.com/r/jomrr/molecule-fedora ) |
| | | 40 | [jomrr/molecule-fedora:40]( https://hub.docker.com/r/jomrr/molecule-fedora ) |
| | | rawhide | [jomrr/molecule-fedora:rawhide]( https://hub.docker.com/r/jomrr/molecule-fedora ) |
| Suse | OpenSuse Leap | 15 | [jomrr/molecule-opensuse-leap:15]( https://hub.docker.com/r/jomrr/molecule-opensuse-leap ) |
| Debian | Ubuntu | 20.04 | [jomrr/molecule-ubuntu:20.04]( https://hub.docker.com/r/jomrr/molecule-ubuntu ) |
| | | 22.04 | [jomrr/molecule-ubuntu:22.04]( https://hub.docker.com/r/jomrr/molecule-ubuntu ) |
| | | 24.04 | [jomrr/molecule-ubuntu:24.04]( https://hub.docker.com/r/jomrr/molecule-ubuntu ) |

## Role Variables

No role default variables specified, see [defaults/main.yml](defaults/main.yml).

## Example Playbook

Example playbooks(s) that show how to use this role.

## Simple example playbook

A simple default example playbook for using jomrr.logindefs.
```yaml
---
# name: "jomrr.logindefs"
# file: "playbook_logindefs.yml"

- name: "PLAYBOOK | logindefs"
  hosts: "logindefs_hosts"
  gather_facts: true
  roles:
    - role: "jomrr.logindefs"
```

## Author(s) and License

- :octocat:                 Author::    [jomrr](https://github.com/jomrr)
- :triangular_flag_on_post: Copyright:: 2024, Jonas Mauer
- :page_with_curl:          License::   [MIT](LICENSE)


---
