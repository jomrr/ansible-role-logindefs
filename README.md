# Ansible Role: logindefs

![GitHub](https://img.shields.io/github/license/jomrr/ansible-role-logindefs)
![GitHub last commit](https://img.shields.io/github/last-commit/jomrr/ansible-role-logindefs)
![GitHub issues](https://img.shields.io/github/issues-raw/jomrr/ansible-role-logindefs)
[![dev](https://img.shields.io/github/actions/workflow/status/jomrr/ansible-role-logindefs/dev.yml?branch=dev&label=dev)](https://github.com/jomrr/ansible-role-logindefs/actions/workflows/dev.yml?query=branch%3Adev)
[![main](https://img.shields.io/github/actions/workflow/status/jomrr/ansible-role-logindefs/main.yml?branch=main&label=main)](https://github.com/jomrr/ansible-role-logindefs/actions/workflows/main.yml?query=branch%3Amain)

Conservatively manage /etc/login.defs.

## Purpose

This role conservatively manages /etc/login.defs without taking ownership of the
distribution authentication stack.

Normal settings only update keys that already exist as active lines.
Missing or commented-only normal settings are skipped.
Force settings are the explicit escape hatch for administrators who
intentionally want to insert or activate a key.
Disabled keys take precedence over force settings, and force settings take
precedence over normal settings.

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

## Dependencies

```yaml
collections:
  - name: community.general
    version: '>=12.0.0'
```

## Role Variables

### `logindefs_settings`

Type: `dict`. Required: `false`.

Normal /etc/login.defs settings.
These settings are changed only when the key already exists as an active line in
/etc/login.defs.
Missing keys and commented-only keys are skipped and are not inserted or
activated.
ENCRYPT_METHOD is intentionally forbidden here and must be configured through
logindefs_force_settings.
Force settings and disabled keys take precedence over normal settings.

Default:

```yaml
logindefs_settings:
  LOGIN_TIMEOUT: '60'
  LOGIN_RETRIES: '5'
  FAILLOG_ENAB: 'yes'
  LOG_OK_LOGINS: 'yes'
  LOG_UNKFAIL_ENAB: 'no'
  SYSLOG_SU_ENAB: 'yes'
  SYSLOG_SG_ENAB: 'yes'
  PASS_MAX_DAYS: '384'
  PASS_MIN_DAYS: '1'
  PASS_WARN_AGE: '7'
  UMASK: '077'
  HOME_MODE: '0700'
  DEFAULT_HOME: 'no'
  TTYPERM: '0600'
  CHFN_RESTRICT: rwh
  SHA_CRYPT_MIN_ROUNDS: '1000000'
  SHA_CRYPT_MAX_ROUNDS: '1000000'
  YESCRYPT_COST_FACTOR: '7'
```

### `logindefs_force_settings`

Type: `dict`. Required: `false`.

Explicit force settings for /etc/login.defs.
These settings are inserted when missing and activated or replaced when
commented or active lines already exist.
Use this escape hatch only when the key is valid and desired on the target host
or distribution.
ENCRYPT_METHOD may only be set here and is limited to SHA256, SHA512, or
YESCRYPT.
Disabled keys take precedence over force settings.

Default:

```yaml
logindefs_force_settings: {}
```

### `logindefs_disabled`

Type: `list`. Required: `false`.

Keys whose active /etc/login.defs lines should be commented out.
Disabled keys are not activated or inserted by either settings dictionary.
This role does not disable HUSHLOGIN_FILE by default because that is site
policy, not a universal security default.

Default:

```yaml
logindefs_disabled: []
```

## Managed Files

- `/etc/login.defs`

## Check Mode

The role uses idempotent Ansible modules and supports check mode for file
changes.

- When /etc/login.defs is absent on SUSE, check mode reports the vendor-file
  bootstrap and skips edits that require the new file.
- Run check mode again after the first convergence to preview individual setting
  changes.

## Service Behavior

The role does not restart or reload services.

## Security Notes

- ENCRYPT_METHOD is not set by default. Hash algorithm changes must be requested
  explicitly through logindefs_force_settings.
- YESCRYPT_COST_FACTOR and SHA_CRYPT_*_ROUNDS can be managed as normal settings.
- LOG_UNKFAIL_ENAB defaults to no to avoid logging secrets accidentally entered
  in the username field.
- PASS_MIN_LEN is not used as a secure default because password quality belongs
  to PAM or pwquality policy.

## Operational Notes

- Repeated runs with unchanged inputs are idempotent, including overrides of
  normal settings through force settings.
- On SUSE, a missing /etc/login.defs is initialized from /usr/etc/login.defs.
  Existing files are preserved.
- Shadow 4.20 removed PASS_MIN_DAYS. Normal settings leave this key absent on
  systems whose vendor configuration no longer provides it.
- Force settings may insert keys that are invalid for a target distribution, so
  they should be used deliberately.

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

## References

- [Shadow 4.20 changes](https://github.com/shadow-maint/shadow/issues/1451)

## Author

[Jonas Mauer](https://github.com/jomrr)

## License

This project is licensed under the MIT License.
See [LICENSE](LICENSE) for the full license text.

Copyright (c) 2024-2026 Jonas Mauer.
