# SSH Role

This Ansible role configures SSH server security settings for enhanced system access control and hardening.

## Description

**Note: This role is currently in development or placeholder state.**

SSH (Secure Shell) is the primary method for secure remote access to systems. This role is intended to:

- Configure SSH server security settings
- Disable insecure authentication methods
- Set up key-based authentication
- Configure connection limits and timeouts
- Implement SSH hardening best practices
- Manage SSH keys and authorized_keys files

## Requirements

- Linux system with SSH server (openssh-server)
- sudo privileges for configuration changes

## Variables

Variables for this role will be documented once implementation is complete.

| Variable | Default | Description |
|----------|---------|-------------|
| TBD | TBD | SSH configuration options to be defined |

## Example Usage

### Basic Usage

```yaml
- hosts: servers
  become: yes
  roles:
    - ssh
```

### In Playbook

```yaml
---
- name: Configure SSH Security
  hosts: bastion
  become: yes
  roles:
    - ssh
```

## Files and Directories

Configuration files that will be managed by this role:

- `/etc/ssh/sshd_config` - Main SSH server configuration
- `/etc/ssh/ssh_config` - SSH client configuration
- `/home/*/.ssh/authorized_keys` - User SSH public keys
- `/etc/ssh/` - SSH configuration directory

## Security Features (Planned)

### SSH Hardening
- Disable root login
- Disable password authentication
- Configure key-based authentication only
- Set strong ciphers and key exchange algorithms
- Configure connection limits

### Access Control
- User/group access restrictions
- IP-based access controls
- SSH key management
- Banner configuration

## Service Management

### Manual Service Commands

```bash
# Start/stop/restart
sudo systemctl start sshd
sudo systemctl stop sshd
sudo systemctl restart sshd

# Check status
sudo systemctl status sshd

# Test configuration
sudo sshd -t

# View logs
sudo journalctl -u sshd -f
```

## Notes

- This role is currently a placeholder and needs implementation
- SSH configuration changes can lock you out of the system
- Always test SSH changes carefully with an active session
- Keep a backup connection open when making SSH changes
- Consider console access for recovery

## TODO

- Implement SSH hardening configurations
- Add key management tasks
- Create security templates
- Add configuration validation
- Implement comprehensive testing 