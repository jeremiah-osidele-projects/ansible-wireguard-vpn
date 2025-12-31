# Ansible WireGuard VPN (Deployed to Raspberry Pi)

This project uses Ansible to configure a hardened WireGuard VPN server on an existing Linux host (e.g. Raspberry Pi). 
Designed with security, idempotency, and maintainability in mind.

## Scope

- Configuration management only
- Host is assumed to already exist and be reachable via SSH
- Infrastructure provisioning is intentionally out of scope

## Features

- Idempotent Ansible roles
- Clear separation of concerns
- Secure key handling via Ansible Vault
- Automatic NAT & IP forwarding
- UFW firewall hardening
- Unattended OS security updates

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
[ Raspberry Pi 5 ]
  - wg0: 10.8.0.1
  - wlan0 → Internet
  - UFW firewall
  - NAT (MASQUERADE)
  ```

## Repository Structure

```
inventory/
├── hosts.ini
├── group_vars/
│   └── vpn/
│       ├── wireguard.yml
│       └── vault.yml (encrypted)
roles/
├── common/
├── wireguard/
playbooks/
└── bootstrap.yml
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
* Private keys are encrypted using Ansible Vault
* Firewall rules restrict access to SSH and WireGuard only
* IP forwarding and NAT are explicitly controlled

## Future Improvements
* Client config auto-generation
* Multiple peer support
* Monitoring & alerts