# Ansible Bastion Host & Reverse Proxy

A comprehensive Ansible playbook for deploying and configuring a secure bastion host with reverse proxy capabilities on Arch Linux systems. This project provides a hardened network entry point with intrusion detection, SSL termination, and comprehensive security monitoring.

## Overview

This Ansible project automates the deployment of a secure bastion host that serves as:

- **Reverse Proxy**: SSL termination and traffic forwarding using Nginx
- **Security Gateway**: Network filtering and access control
- **Monitoring Platform**: Intrusion detection and security auditing
- **Certificate Authority**: Self-signed PKI for SSL/TLS certificates


### Key Components

- **Nginx**: Reverse proxy with SSL termination
- **PKI**: Self-signed certificate authority and server certificates
- **Firewall**: iptables-based network filtering
- **Fail2ban**: Intrusion prevention system
- **Suricata**: Network intrusion detection system
- **Auditd**: System call auditing and monitoring

## Roles

| Role | Purpose | Status |
|------|---------|---------|
| **yay** | AUR helper for Arch Linux package management | ✅ Complete |
| **pki** | Certificate Authority and SSL certificate generation | ✅ Complete |
| **nginx** | Reverse proxy with SSL termination | ✅ Complete |
| **firewall** | iptables firewall configuration | ✅ Complete |
| **fail2ban** | Intrusion prevention and IP banning | ✅ Complete |
| **suricata** | Network intrusion detection system | ✅ Complete |
| **auditd** | System auditing and monitoring | ✅ Complete |

## Ansible Requirements
- **Ansible**
- **Python**
- **SSH Access**: Key-based authentication to target hosts
- **Privileges**: sudo access on target systems


## Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/Anselm98/ansible-bastion.git
cd ansible-bastion
```

### 2. Configure Inventory

Edit the inventory file to specify your target hosts:

```ini
[bastion]
bastion-host ansible_host=192.168.1.100 ansible_user=admin

[bastion:vars]
suricata_interface=eth0
backend_server=192.168.187.200
backend_port=443
```

### 3. Configure Variables

Edit `group_vars/all.yml`

### 4. Run the Playbook

```bash
# Deploy complete bastion host
ansible-playbook -i inventory/hosts playbooks/site.yml
```

## Playbook

### Complete Bastion Deployment

```yaml
---
- name: Deploy Secure Bastion Host
  hosts: bastion
  become: yes
  roles:
    - yay          # AUR helper
    - pki          # SSL certificates
    - firewall     # Network filtering
    - fail2ban     # Intrusion prevention
    - suricata     # Network monitoring
    - auditd       # System auditing
    - nginx        # Reverse proxy
```
## Directory Structure

```
ansible-bastion/
├── README.md              # This file
├── ansible.cfg            # Ansible configuration
├── inventory/             # Host inventories
├── group_vars/            # Group variables
├── playbooks/             # Ansible playbooks
├── roles/                 # Ansible roles
    ├── auditd/              # System auditing
    ├── fail2ban/            # Intrusion prevention
    ├── firewall/            # Network filtering
    ├── nginx/               # Reverse proxy
    ├── pki/                 # Certificate management
    ├── suricata/            # Network IDS
    └── yay/                 # AUR helper

```

## Security Features

### Network Security
- **Firewall**: iptables-based packet filtering
- **IDS/IPS**: Suricata network monitoring and Fail2ban intrusion prevention
- **SSL/TLS**: PKI-generated certificates with strong encryption

### System Security
- **Auditing**: Comprehensive system call monitoring
- **Access Control**: SSH hardening and key-based authentication
- **Log Management**: Centralized logging and rotation

### Monitoring
- **Real-time Alerts**: Suricata IDS alerts
- **Attack Prevention**: Automatic IP banning via Fail2ban
- **Audit Trail**: Complete system activity logging