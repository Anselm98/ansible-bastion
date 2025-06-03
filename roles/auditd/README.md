# Auditd Role

This Ansible role installs and configures auditd (Linux Audit System) on Linux systems for security auditing and monitoring.

## Description

The Linux Audit System provides a way to track security-relevant information on your system. This role:

- Installs the audit package and utilities
- Configures auditd main configuration
- Sets up custom audit rules for monitoring
- Configures log rotation for audit logs
- Enables and starts the auditd service

## Requirements

- Linux system with systemd
- sudo privileges for installation and configuration

## Variables

This role uses template files for configuration. The following variables can be customized in your inventory or playbook:

| Variable | Default | Description |
|----------|---------|-------------|
| Various auditd configuration options | See templates | Configured via template variables |

## Example Usage

### Basic Usage

```yaml
- hosts: servers
  become: yes
  roles:
    - auditd
```

### In Playbook

```yaml
---
- name: Configure Security Auditing
  hosts: bastion
  become: yes
  roles:
    - auditd
```

## Files and Directories

After installation, the following files and directories are configured:

- `/etc/audit/auditd.conf` - Main auditd configuration
- `/etc/audit/rules.d/bastion.rules` - Custom audit rules
- `/etc/logrotate.d/audit` - Log rotation configuration

## Service Management

### Manual Service Commands

```bash
# Start/stop/restart
sudo systemctl start auditd
sudo systemctl stop auditd
sudo systemctl restart auditd

# Check status
sudo systemctl status auditd

# View logs
sudo journalctl -u auditd -f
```

## Log Files

Audit logs are stored in:
- `/var/log/audit/audit.log` - Main audit log file

## Notes

- This role is idempotent and can be run multiple times safely
- The auditd service requires special handling for restarts
- Custom audit rules are placed in `/etc/audit/rules.d/` for better organization
- Log rotation is automatically configured to manage audit log file sizes 