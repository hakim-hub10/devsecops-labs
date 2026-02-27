# Server Security Hardening Report

## Overview

This document summarizes the security hardening performed on an Ubuntu 24.04 LTS server hosted on Hetzner Cloud.

The goal was to reduce attack surface, eliminate public SSH exposure, and implement secure remote administration using VPN-based access.

---

## 1. Initial Server Setup

- Provider: Hetzner Cloud
- OS: Ubuntu 24.04 LTS
- Kubernetes: k3s (single-node)
- Docker & Docker Compose installed

---

## 2. Account Security (Hetzner)

- Enabled Two-Factor Authentication (2FA) via Google Authenticator
- Disabled password-based login on server
- Root login disabled
- SSH key-based authentication enforced

Impact:
- Prevents credential stuffing
- Protects against phishing-based account takeover
- Eliminates password brute force on SSH

---

## 3. Firewall Hardening (UFW)

UFW configured with default deny incoming policy.

Allowed ports:
- 8080/tcp (Web service)
- 51820/udp (WireGuard VPN)

Restricted:
- SSH (22/tcp) only allowed from VPN subnet (10.10.0.0/24)

Removed:
- All public SSH access (IPv4 and IPv6)

Impact:
- Eliminates global SSH exposure
- Reduces attack surface significantly
- Prevents automated brute-force attacks

---

## 4. Fail2ban Configuration

Fail2ban installed and active.

- Monitors SSH login attempts
- Automatically bans brute-force IP addresses
- Verified active jails and bans

Impact:
- Mitigates repeated login attempts
- Protects against credential guessing attacks

---

## 5. WireGuard VPN Implementation

WireGuard deployed for secure administrative access.

Server:
- VPN network: 10.10.0.0/24
- Server IP: 10.10.0.1
- Client IP: 10.10.0.2
- Port: 51820/udp

SSH access now only possible via:
Client → WireGuard → Internal IP (10.10.0.1)

Public SSH completely disabled.

Impact:
- SSH hidden from public internet
- Eliminates port scanning exposure
- Establishes secure administrative network plane

---

## 6. Attack Surface Reduction

Before:
- SSH publicly accessible
- Continuous brute-force attempts in logs

After:
- No public SSH exposure
- Only VPN-authenticated access
- Reduced attack noise

---

## 7. Security Improvements Achieved

- Private administrative network
- Zero public SSH surface
- 2FA-protected cloud account
- Active intrusion mitigation (Fail2ban)
- Controlled firewall rules
- Reduced bot traffic

---

## Conclusion

The server now follows a hardened access model:

- Public services exposed only when required
- Administrative access isolated behind VPN
- Strong authentication enforced at both cloud and system level

This configuration significantly reduces attack surface and aligns with production-grade infrastructure security practices.