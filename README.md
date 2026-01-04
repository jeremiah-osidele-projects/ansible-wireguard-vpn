# Ansible WireGuard VPN (Deployed to Raspberry Pi)

This project uses Ansible to configure a hardened WireGuard VPN server on an existing Linux host (e.g. Raspberry Pi). 
Designed with security, idempotency, and maintainability in mind.

## Scope

- Configuration management only
- Host is assumed to already exist and be reachable via SSH
- Infrastructure provisioning is intentionally out of scope

## Design Decisions

* Configuration as Code: Entire setup can be redeployed to a fresh host
* No provisioning logic: Keeps focus on systems configuration
* Handlers for service restarts: Prevents unnecessary downtime
* Cron-based DDNS: Simple, reliable, and auditable
* Single responsibility roles: Improves readability and maintainability

## Features

- Idempotent Ansible roles
- Clear separation of concerns
- Secure key handling via Ansible Vault
- Automatic NAT & IP forwarding
- UFW firewall hardening
- Unattended OS security updates
- CI linting via GitHub Actions

## Technologies

- Ansible
- WireGuard
- Linux (Debian-based)

## Architecture

```
Laptop / Phone
    |
    |  WireGuard (10.8.0.0/24)
    |
[ Raspberry Pi 5 - VPN Host ]
  - wg0: 10.8.0.1
  - wlan0 Home LAN / Internet
  - UFW firewall rules
  - NAT (MASQUERADE)
  - DDNS:    DuckDNS updates
  ```

## Repository Structure

```
inventory/
├── hosts.ini
├── group_vars/
│   └── vpn/
│       ├── wireguard.yml
│       └── vault.yml          # Encrypted secrets
roles/
├── common/                    # OS baseline & hardening
├── wireguard/                 # VPN, firewall, NAT
├── ddns/                      # Dynamic DNS automation
playbooks/
└── bootstrap.yml              # Main entrypoint
```

## Usage
Dry run
```
ansible-playbook playbooks/bootstrap.yml --ask-pass --ask-become-pass --ask-vault-pass --check
```

Apply
```
ansible-playbook playbooks/bootstrap.yml --ask-pass --ask-become-pass --ask-vault-pass
```

## Security Notes
* Private keys are encrypted using Ansible Vault, and never store in plain text
* Firewall rules restrict access to SSH and WireGuard only
* IP forwarding and NAT are explicitly controlled
* DDNS update script runs with least privilege
* Service restarts are handled via Ansible handlers to avoid unnecessary disruption

## Future Improvements
* Client config auto-generation
* Multiple peer support
* Monitoring & alerts