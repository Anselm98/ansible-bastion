# Yay AUR Helper Role

This Ansible role installs and configures the `yay` AUR (Arch User Repository) helper on Arch Linux systems. This role is designed to be used as a dependency for other roles that need to install packages from the AUR.

## Purpose

- Installs required build dependencies (`base-devel`, `git`)
- Creates a dedicated user for building AUR packages
- Configures sudo permissions for the AUR builder user
- Installs the `yay` AUR helper
- Provides a clean, reusable way to ensure AUR access across multiple roles

## Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `yay_builder_user` | `aur_builder` | Username for the non-root user that builds AUR packages |

## Dependencies

None. This role is designed to be a base dependency for other roles.

## Example Usage

### As a standalone role:
```yaml
- hosts: all
  roles:
    - yay
```

### As a dependency in meta/main.yml:
```yaml
dependencies:
  - role: yay
```

### In a playbook with custom user:
```yaml
- hosts: all
  roles:
    - role: yay
      vars:
        yay_builder_user: custom_builder
```

## Notes

- This role is idempotent - it will only install `yay` if it's not already present
- The AUR builder user is created as a system user
- The role cleans up the build directory after installation
- Only runs on Arch Linux systems where `yay` is not already installed 