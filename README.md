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
| RedHat | AlmaLinux | latest | [jomrr/molecule-almalinux:latest]( https://hub.docker.com/r/jomrr/molecule-almalinux ) |
| Archlinux | Archlinux | latest | [jomrr/molecule-archlinux:latest]( https://hub.docker.com/r/jomrr/molecule-archlinux ) |
| Debian | Debian | latest | [jomrr/molecule-debian:latest]( https://hub.docker.com/r/jomrr/molecule-debian ) |
| RedHat | Fedora | latest | [jomrr/molecule-fedora:latest]( https://hub.docker.com/r/jomrr/molecule-fedora ) |
| Suse | OpenSuse Leap | latest | [jomrr/molecule-opensuse-leap:latest]( https://hub.docker.com/r/jomrr/molecule-opensuse-leap ) |
| Debian | Ubuntu | latest | [jomrr/molecule-ubuntu:latest]( https://hub.docker.com/r/jomrr/molecule-ubuntu ) |

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
