                           Internet
                               │
                               │
                      Public IP (eth0)
                               │
                 ┌─────────────┴─────────────┐
                 │                           │
           Port 8080                    Port 51820 (UDP)
         Public Web App                 WireGuard VPN
                 │                           │
                 │                           │
                 │                    Encrypted Tunnel
                 │                           │
                 │                     10.10.0.0/24
                 │                           │
                 │                     10.10.0.1
                 │                           │
                 │                        SSH (22)
                 │                           │
                 └──────────────►   Private Admin Access


---

## Access Flow

### Public Traffic
Internet → eth0 → Port 8080 → Web Service

### Administrative Access
Admin Device → WireGuard VPN → 10.10.0.1 → SSH

SSH is NOT publicly exposed.

---

## Security Layers

1. Cloud Account Protection (2FA)
2. Firewall Enforcement (UFW)
3. SSH Hardening (No password, no root login)
4. VPN Isolation (WireGuard)
5. Intrusion Mitigation (Fail2ban)

---

## Network Segmentation Model

- Public plane: Web services
- Control plane: VPN-based administrative access
- Host protection: Firewall + SSH policy

This architecture enforces separation between public exposure and administrative access.