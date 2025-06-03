# Fail2ban Role

This Ansible role installs and configures Fail2ban for intrusion prevention by monitoring log files and banning IPs with malicious behavior.

## Description

Fail2ban scans log files and bans IPs that show malicious signs. This role:

- Installs the fail2ban package
- Creates necessary directories for configuration
- Configures jail settings for SSH and Nginx
- Sets up custom filters for Nginx protection
- Enables and starts the fail2ban service

## Requirements

- Linux system with systemd
- sudo privileges for installation and configuration
- Nginx (for web-related jails)
- SSH daemon (for SSH protection)

## Variables

This role uses template files for configuration. The following variables can be customized in your inventory or playbook:

| Variable | Default | Description |
|----------|---------|-------------|
| Various fail2ban configuration options | See templates | Configured via template variables |

## Example Usage

### Basic Usage

```yaml
- hosts: servers
  become: yes
  roles:
    - fail2ban
```

### In Playbook

```yaml
---
- name: Configure Intrusion Prevention
  hosts: bastion
  become: yes
  roles:
    - fail2ban
```

## Files and Directories

After installation, the following files and directories are configured:

- `/etc/fail2ban/jail.local` - Main jail configuration
- `/etc/fail2ban/jail.d/nginx.conf` - Nginx jail configuration
- `/etc/fail2ban/jail.d/sshd.conf` - SSH jail configuration
- `/etc/fail2ban/filter.d/nginx-http-auth.conf` - Nginx HTTP auth filter
- `/etc/fail2ban/filter.d/nginx-noscript.conf` - Nginx noscript filter
- `/etc/fail2ban/filter.d/nginx-badbots.conf` - Nginx bad bots filter

## Protection Features

### SSH Protection
- Monitors SSH authentication failures
- Bans IPs with repeated failed login attempts

### Nginx Protection
- HTTP authentication failures
- Script injection attempts
- Bad bot detection
- Custom filter configurations

## Service Management

### Manual Service Commands

```bash
# Start/stop/restart
sudo systemctl start fail2ban
sudo systemctl stop fail2ban
sudo systemctl restart fail2ban

# Check status
sudo systemctl status fail2ban

# View logs
sudo journalctl -u fail2ban -f
```

### Fail2ban Commands

```bash
# Check jail status
sudo fail2ban-client status

# Check specific jail
sudo fail2ban-client status sshd

# Unban an IP
sudo fail2ban-client set sshd unbanip <IP_ADDRESS>

# List banned IPs
sudo fail2ban-client get sshd banip
```

## Log Files

Fail2ban logs are available in:
- `/var/log/fail2ban.log` - Main fail2ban log file
- Journal logs via `journalctl -u fail2ban`

## Notes

- This role is idempotent and can be run multiple times safely
- Jails are configured for both SSH and Nginx by default
- Custom filters are included for enhanced Nginx protection
- Backup files are created when updating configurations
- The role creates all necessary directory structures 