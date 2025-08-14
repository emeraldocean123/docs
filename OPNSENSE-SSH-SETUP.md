# OPNsense SSH Configuration

## System Information
- **Hostname**: pve-opnsense-vm-router.lan
- **IP Address**: 192.168.1.1
- **OS Version**: OPNsense 25.7.1_1 (amd64)
- **Platform**: FreeBSD 14.3-RELEASE-p1
- **Location**: VM on Proxmox host (192.168.1.40)

## Network Configuration
- **LAN Interface (igc0)**: 192.168.1.1/24
- **WAN Interface (igc1)**: DHCP assigned (98.51.188.39/23)

## SSH Access Configuration

### User Accounts
Both accounts have the unified SSH key configured:

1. **joseph** (Regular user)
   - Member of wheel group for sudo access
   - Recommended for daily use
   - Home directory: `/home/joseph`
   - Shell: `/bin/sh`

2. **root** (Administrator)
   - Use only when administrative access required
   - Home directory: `/root`

### SSH Key Configuration
- **Key Type**: ED25519
- **Key Location**: `~/.ssh/id_ed25519_unified`
- **Public Key**: `ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEBdb5WWyH4atlYewmthJGTVAkJysN3UHp5ZhUDtfbp2 joseph@unified-key`
- **Fingerprint**: `SHA256:2Oa0aH7dI5P+5H7wHTn5gsKWfHUw2oM5POWsBgP7AH8`

### SSH Connection Methods

#### Using SSH Alias (Recommended)
```bash
ssh opnsense    # Connects as joseph user
```

#### Direct Connection
```bash
ssh joseph@192.168.1.1    # Regular user access
ssh root@192.168.1.1      # Root access (when needed)
```

### SSH Config Entry
Located in `~/.ssh/config`:
```
Host opnsense
    HostName 192.168.1.1
    User joseph
    IdentityFile ~/.ssh/id_ed25519_unified
```

## Setup Date
- **Created**: August 14, 2025
- **joseph user created**: August 14, 2025
- **Unified key added**: August 14, 2025

## Security Notes
- Password authentication is enabled as fallback
- joseph user is in wheel group for administrative tasks via sudo
- Both accounts secured with the same unified SSH key
- Recommend using joseph account for regular access

## OPNsense Web Interface
- **HTTPS Access**: https://192.168.1.1
- **Certificate SHA256**: `29 08 F9 58 7F FF 8E 3C 5C 95 EF 8C 88 8C 6A 17 13 BC 67 BE B4 83 52 63 C0 E7 C0 54 87 35 52 B3`

## Host SSH Fingerprints
- **ECDSA**: `SHA256:VuIqH337lssbOzvG+5bU9/oWCBpd4scHdfr5o//921A`
- **ED25519**: `SHA256:yrNJM8Xnd99oxgPkVZ+HtWW4d0nHj/YHOyjW7WXkoTQ`
- **RSA**: `SHA256:CMsOEw7iH9AqQG9ecoot3Wbj/39lInZVmCUAdeIHq4o`