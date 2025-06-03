# Firewall Role

This Ansible role configures iptables firewall rules to secure the system by controlling network traffic.

## Description

This role manages iptables configuration to provide network security. It:

- Installs iptables package
- Creates iptables configuration directory
- Backs up existing iptables rules
- Deploys custom iptables rules from template
- Creates and enables systemd service for iptables persistence
- Applies firewall rules immediately

## Requirements

- Linux system with iptables support
- systemd for service management
- sudo privileges for installation and configuration

## Variables

This role uses template files for firewall rules configuration. The following variables can be customized in your inventory or playbook:

| Variable | Default | Description |
|----------|---------|-------------|
| Various iptables rules | See templates | Configured via iptables.rules.j2 template |

## Example Usage

### Basic Usage

```yaml
- hosts: servers
  become: yes
  roles:
    - firewall
```

### In Playbook

```yaml
---
- name: Configure Network Firewall
  hosts: bastion
  become: yes
  roles:
    - firewall
```

## Files and Directories

After installation, the following files and directories are configured:

- `/etc/iptables/` - Configuration directory
- `/etc/iptables/iptables.rules` - Active firewall rules
- `/etc/iptables/iptables.rules.backup` - Backup of previous rules
- `/etc/systemd/system/iptables.service` - Systemd service file

## Service Management

### Manual Service Commands

```bash
# Start/stop/restart
sudo systemctl start iptables
sudo systemctl stop iptables
sudo systemctl restart iptables

# Check status
sudo systemctl status iptables

# View current rules
sudo iptables -L -n -v
```

### Manual Rule Management

```bash
# View current rules
sudo iptables -L

# Save current rules
sudo iptables-save > /etc/iptables/iptables.rules

# Restore rules from file
sudo iptables-restore < /etc/iptables/iptables.rules

# Flush all rules (CAUTION!)
sudo iptables -F
```

## Firewall Configuration

The firewall rules are configured via the `iptables.rules.j2` template, which typically includes:

- Default policies for chains
- Loopback interface rules
- Connection state rules
- Service-specific port rules
- Logging and drop rules

## Backup and Safety

- Existing rules are automatically backed up before changes
- Rules are applied immediately after configuration
- Service is enabled to persist rules across reboots
- Systemd service ensures rules are loaded at boot time

## Notes

- This role is idempotent and can be run multiple times safely
- Always test firewall rules carefully to avoid locking yourself out
- The role includes backup functionality to help recover from misconfigurations
- Rules are applied immediately and persisted across reboots
- Custom iptables rules should be defined in the template files 