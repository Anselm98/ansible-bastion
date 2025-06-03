# Nginx Role

This Ansible role installs and configures Nginx as a reverse proxy for secure web traffic forwarding and SSL termination.

## Description

This role sets up Nginx as a reverse proxy server for handling web traffic. It:

- Installs the nginx package
- Configures main nginx configuration
- Sets up reverse proxy configuration
- Creates necessary directory structures
- Enables and starts the nginx service

## Requirements

- Linux system with systemd
- sudo privileges for installation and configuration
- SSL certificates (can be provided by PKI role)

## Variables

This role uses the following variables that can be customized in your inventory or playbook:

| Variable | Default | Description |
|----------|---------|-------------|
| `nginx_ssl_port` | `443` | SSL port for Nginx to listen on |
| `backend_server` | `192.168.187.200` | Backend server IP for reverse proxy |
| `backend_port` | `443` | Backend server port for reverse proxy |

## Example Usage

### Basic Usage

```yaml
- hosts: servers
  become: yes
  roles:
    - nginx
```

### With Custom Backend

```yaml
- hosts: servers
  become: yes
  roles:
    - role: nginx
      vars:
        backend_server: "10.0.1.100"
        backend_port: 8080
        nginx_ssl_port: 8443
```

### In Playbook

```yaml
---
- name: Configure Reverse Proxy
  hosts: bastion
  become: yes
  roles:
    - pki  # For SSL certificates
    - nginx
```

## Files and Directories

After installation, the following files and directories are configured:

- `/etc/nginx/nginx.conf` - Main nginx configuration
- `/etc/nginx/conf.d/default.conf` - Reverse proxy configuration
- `/etc/nginx/conf.d/` - Additional configuration directory

## Service Management

### Manual Service Commands

```bash
# Start/stop/restart
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx

# Reload configuration
sudo systemctl reload nginx

# Check status
sudo systemctl status nginx

# Test configuration
sudo nginx -t

# View logs
sudo journalctl -u nginx -f
```

## Configuration Features

### Reverse Proxy
- Forwards traffic to backend servers
- SSL termination at the proxy level
- Custom backend server configuration

### Security
- SSL/TLS configuration
- Integration with PKI role for certificates
- Secure headers and configurations

## Log Files

Nginx logs are stored in:
- `/var/log/nginx/access.log` - Access logs
- `/var/log/nginx/error.log` - Error logs

## Dependencies

This role works well with:
- `pki` role - For SSL certificate generation
- `fail2ban` role - For intrusion protection
- `firewall` role - For network security

## Notes

- This role is idempotent and can be run multiple times safely
- Configuration files are backed up before changes
- The role creates necessary directory structures automatically
- SSL certificates should be provided by the PKI role or configured separately
- Default configuration sets up a reverse proxy to a backend server 