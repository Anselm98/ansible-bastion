# Suricata IDS/IPS Role for Arch Linux

This Ansible role installs and configures Suricata IDS/IPS on Arch Linux systems following the [official Suricata documentation](https://docs.suricata.io/en/latest/install.html).

## Description

Suricata is a high performance Network IDS, IPS and Network Security Monitoring engine. This role:

- Installs Suricata from the Arch User Repository (AUR) using `yay`
- Creates and configures the suricata user and required directories
- Deploys a customizable Suricata configuration
- Sets up rule management with `suricata-update`
- Configures systemd service with proper network interface binding
- Validates configuration before starting the service

## Dependencies

- **yay role**: This role depends on the `yay` role for AUR package installation

## Requirements

- Arch Linux system
- Network interface name for monitoring
- Sudo privileges for installation and configuration

## Variables

### Required Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `suricata_interface` | Network interface to monitor | `eth0`, `enp0s3` |

### Optional Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `suricata_user` | `suricata` | System user for Suricata |
| `suricata_group` | `suricata` | System group for Suricata |
| `suricata_config_dir` | `/etc/suricata` | Configuration directory |
| `suricata_log_dir` | `/var/log/suricata` | Log directory |
| `suricata_lib_dir` | `/var/lib/suricata` | State/rules directory |
| `suricata_home_net` | `[192.168.0.0/16,10.0.0.0/8,172.16.0.0/12]` | Internal network definition |
| `suricata_external_net` | `!$HOME_NET` | External network definition |
| `suricata_update_rules` | `true` | Whether to update rules automatically |
| `suricata_service_enabled` | `true` | Enable service on boot |
| `suricata_service_state` | `started` | Service state |
| `suricata_profile` | `medium` | Performance profile |
| `suricata_log_level` | `notice` | Logging level |

## Example Usage

### Basic Usage

```yaml
- hosts: security_servers
  become: yes
  roles:
    - role: suricata
      vars:
        suricata_interface: eth0
```

### Advanced Configuration

```yaml
- hosts: security_servers
  become: yes
  roles:
    - role: suricata
      vars:
        suricata_interface: enp0s3
        suricata_home_net: "[10.0.0.0/8,172.16.0.0/12]"
        suricata_profile: high
        suricata_update_rules: true
```

### In Playbook

```yaml
---
- name: Configure Network Security Monitoring
  hosts: bastion
  become: yes
  roles:
    - yay
    - suricata
  vars:
    suricata_interface: "{{ ansible_default_ipv4.interface }}"
```

## Directory Structure

After installation, the following directories are created:

- `/etc/suricata/` - Configuration files
- `/var/log/suricata/` - Log files (owned by suricata user)
- `/var/lib/suricata/` - Rules and state files (owned by suricata user)
- `/var/lib/suricata/rules/` - Rule files

## Service Management

The role automatically:

1. Validates the configuration before starting
2. Configures systemd service with proper interface binding
3. Enables automatic rule updates via `suricata-update`
4. Sets up proper log rotation

### Manual Service Commands

```bash
# Start/stop/restart
sudo systemctl start suricata
sudo systemctl stop suricata
sudo systemctl restart suricata

# Reload rules without stopping
sudo systemctl reload suricata

# Check status
sudo systemctl status suricata

# View logs
sudo journalctl -u suricata -f
```

## Rule Management

The role automatically installs and configures `suricata-update` for rule management:

```bash
# Manual rule update
sudo suricata-update

# Check rule sources
sudo suricata-update list-sources
```

## Notes

- This role follows the official Arch Linux installation method using AUR
- Suricata runs as a non-privileged system user
- Configuration is validated before service start
- The role is idempotent and can be run multiple times safely
- EVE JSON logging is enabled by default for integration with log analysis tools

## Troubleshooting

### Check Configuration

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml
```

### Monitor Interface

Ensure the specified interface exists and is up:

```bash
ip link show {{ suricata_interface }}
```

### View Live Alerts

```bash
sudo tail -f /var/log/suricata/fast.log
``` 