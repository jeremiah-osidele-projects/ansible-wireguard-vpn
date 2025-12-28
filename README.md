# Ansible WireGuard VPN

This project uses Ansible to configure a hardened WireGuard VPN server on an existing Linux host (e.g. Raspberry Pi).

## Scope

- Configuration management only
- Host is assumed to already exist and be reachable via SSH
- Infrastructure provisioning is intentionally out of scope

## Goals

- Reproducible VPN setup
- Secure defaults
- Idempotent automation
- Clear separation of concerns

## Technologies

- Ansible
- WireGuard
- Linux (Debian-based)

