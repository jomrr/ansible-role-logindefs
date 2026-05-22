# Ansible Role: logindefs

![GitHub](https://img.shields.io/github/license/jomrr/ansible-role-logindefs) ![GitHub last commit](https://img.shields.io/github/last-commit/jomrr/ansible-role-logindefs) ![GitHub issues](https://img.shields.io/github/issues-raw/jomrr/ansible-role-logindefs) [![dev](https://img.shields.io/github/actions/workflow/status/jomrr/ansible-role-logindefs/dev-push-smoke.yml?branch=dev&event=push&label=dev)](https://github.com/jomrr/ansible-role-logindefs/actions/workflows/dev-push-smoke.yml?query=branch%3Adev) [![main](https://img.shields.io/github/actions/workflow/status/jomrr/ansible-role-logindefs/main-full-gate.yml?branch=main&event=push&label=main)](https://github.com/jomrr/ansible-role-logindefs/actions/workflows/main-full-gate.yml?query=branch%3Amain)

Conservatively manage /etc/login.defs.

## Purpose

This role conservatively manages /etc/login.defs without taking ownership of the distribution authentication stack.

Normal settings only update keys that already exist as active lines.
Missing or commented-only normal settings are skipped.
Force settings are the explicit escape hatch for administrators who intentionally want to insert or activate a key.

## Scope

### Managed

- /etc/login.defs

### Not Managed

- PAM configuration
- authselect profiles
- pwquality configuration
- Password quality policy outside /etc/login.defs
- Existing user accounts
- useradd defaults outside /etc/login.defs
- Distribution-specific authentication stacks

## Role Variables

The following variables are part of the public role interface.

| Name | Required | Default | Description |
| ---- | -------- | ------- | ----------- |
| `logindefs_settings` | False | `LOGIN_TIMEOUT: '60'<br>LOGIN_RETRIES: '5'<br>FAILLOG_ENAB: 'yes'<br>LOG_OK_LOGINS: 'yes'<br>LOG_UNKFAIL_ENAB: 'no'<br>SYSLOG_SU_ENAB: 'yes'<br>SYSLOG_SG_ENAB: 'yes'<br>PASS_MAX_DAYS: '384'<br>PASS_MIN_DAYS: '1'<br>PASS_WARN_AGE: '7'<br>UMASK: '077'<br>HOME_MODE: '0700'<br>DEFAULT_HOME: 'no'<br>TTYPERM: '0600'<br>CHFN_RESTRICT: rwh<br>SHA_CRYPT_MIN_ROUNDS: '1000000'<br>SHA_CRYPT_MAX_ROUNDS: '1000000'<br>YESCRYPT_COST_FACTOR: '7'` | - Normal /etc/login.defs settings.<br>- These settings are changed only when the key already exists as an active line in<br>  /etc/login.defs.<br>- Missing keys and commented-only keys are skipped and are not inserted or activated.<br>- ENCRYPT_METHOD is intentionally forbidden here and must be configured through logindefs_force_settings. |
| `logindefs_force_settings` | False | `{}` | - Explicit force settings for /etc/login.defs.<br>- These settings are inserted when missing and activated or replaced when commented<br>  or active lines already exist.<br>- Use this escape hatch only when the key is valid and desired on the target host<br>  or distribution.<br>- ENCRYPT_METHOD may only be set here and is limited to SHA256, SHA512, or YESCRYPT. |
| `logindefs_disabled` | False | `[]` | - Keys whose active /etc/login.defs lines should be commented out.<br>- This role does not disable HUSHLOGIN_FILE by default because that is site policy,<br>  not a universal security default. |

## Managed Files

- `/etc/login.defs`

## Check Mode

The role uses idempotent Ansible modules and supports check mode for file changes.

## Service Behavior

The role does not restart or reload services.

## Security Notes

- ENCRYPT_METHOD is not set by default. Hash algorithm changes must be requested explicitly through logindefs_force_settings.
- YESCRYPT_COST_FACTOR and SHA_CRYPT_*_ROUNDS can be managed as normal settings.
- LOG_UNKFAIL_ENAB defaults to no to avoid logging secrets accidentally entered in the username field.
- PASS_MIN_LEN is not used as a secure default because password quality belongs to PAM or pwquality policy.

## Operational Notes

- Force settings may insert keys that are invalid for a target distribution, so they should be used deliberately.

## Supported Platforms

| OS Family | Distribution | Version | Container Image |
| --------- | ------------ | ------- | --------------- |
| RedHat | AlmaLinux | latest | [jomrr/molecule-almalinux:latest](https://hub.docker.com/r/jomrr/molecule-almalinux) |
| Archlinux | Archlinux | latest | [jomrr/molecule-archlinux:latest](https://hub.docker.com/r/jomrr/molecule-archlinux) |
| Debian | Debian | latest | [jomrr/molecule-debian:latest](https://hub.docker.com/r/jomrr/molecule-debian) |
| RedHat | Fedora | latest | [jomrr/molecule-fedora:latest](https://hub.docker.com/r/jomrr/molecule-fedora) |
| Suse | OpenSuse Leap | latest | [jomrr/molecule-opensuse-leap:latest](https://hub.docker.com/r/jomrr/molecule-opensuse-leap) |
| Suse | OpenSuse Tumbleweed | latest | [jomrr/molecule-opensuse-tumbleweed:latest](https://hub.docker.com/r/jomrr/molecule-opensuse-tumbleweed) |
| Debian | Ubuntu | latest | [jomrr/molecule-ubuntu:latest](https://hub.docker.com/r/jomrr/molecule-ubuntu) |

## Example Playbook

### Simple example playbook

Minimal example for applying this role.

```yaml
---
- name: "Configure logindefs"
  hosts: "logindefs"
  gather_facts: true
  roles:
    - role: "jomrr.logindefs"
```

## Author

[Jonas Mauer](https://github.com/jomrr)

## License

This project is licensed under the MIT License.
See [LICENSE](LICENSE) for the full license text.

Copyright (c) 2024 Jonas Mauer.
