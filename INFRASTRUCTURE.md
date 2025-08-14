# Infrastructure Overview

## Unified SSH Key
**Fingerprint**: `SHA256:2Oa0aH7dI5P+5H7wHTn5gsKWfHUw2oM5POWsBgP7AH8`
**Location**: `~/.ssh/id_ed25519_unified`

## Systems

### OPNsense Router
| Host | IP | User | Shortcut | Security |
|------|-----|------|----------|----------|
| OPNsense VM | 192.168.1.1 | joseph/root | `ssh opnsense` | FreeBSD 14.3, VM on Proxmox |

### Synology NAS
| Host | IP | User | Shortcut | Security |
|------|-----|------|----------|----------|
| Synology DS1520+ | 192.168.1.10 | joseph | `ssh nas` | Auto Block (10 attempts/5 min) |

### NixOS Laptops
| Host | IP | User | Shortcut | Description |
|------|-----|------|----------|-------------|
| HP dv9500 Pavilion | 192.168.1.104 | joseph | `ssh hp` | NixOS with LXQt desktop |
| MSI GE75 Raider | 192.168.1.106 | joseph | `ssh msi` | NixOS with KDE Plasma 6 |

### Proxmox Infrastructure
| Host | IP | User | Shortcut | Description |
|------|-----|------|----------|-------------|
| Proxmox VE Host | 192.168.1.40 | root | `ssh proxmox` | Main virtualization host |

### LXC Containers
| Container | IP | CTID | Shortcut | Purpose |
|-----------|-----|------|----------|---------|
| WireGuard | 192.168.1.50 | 1000 | `ssh wireguard` | VPN server |
| Tailscale | 192.168.1.51 | 1001 | `ssh tailscale` | Mesh VPN |
| Omada | 192.168.1.52 | 1002 | `ssh omada` | Network controller |
| NetBox | 192.168.1.53 | 1003 | `ssh netbox` | IPAM/DCIM |
| iVentoy | 192.168.1.54 | 1004 | `ssh iventoy` | PXE boot server |
| Docker | 192.168.1.55 | 1005 | `ssh docker` | Container services |
| Syncthing | 192.168.1.56 | 1006 | `ssh syncthing` | File synchronization |

## Quick Commands

### Connect to any system
```bash
# NAS
ssh nas         # Synology NAS

# Laptops
ssh hp          # HP laptop
ssh msi         # MSI laptop

# Proxmox
ssh proxmox     # Main host
ssh wireguard   # WireGuard VPN
ssh tailscale   # Tailscale VPN
ssh omada       # Network controller
ssh netbox      # IPAM
ssh iventoy     # PXE boot
ssh docker      # Docker host
ssh syncthing   # File sync
```

### Manage LXCs from Proxmox
```bash
# List all containers
ssh proxmox "pct list"

# Start/stop container
ssh proxmox "pct start 1000"  # Start WireGuard
ssh proxmox "pct stop 1000"   # Stop WireGuard

# Execute command in container
ssh proxmox "pct exec 1000 -- hostname"

# Enter container console
ssh proxmox "pct enter 1000"
```

### Check system status
```bash
# Check all systems connectivity
for host in nas hp msi proxmox wireguard tailscale omada netbox iventoy docker syncthing; do
    echo -n "$host: "
    ssh -o ConnectTimeout=2 $host "hostname" 2>/dev/null || echo "offline"
done

# Or use the PowerShell test script
powershell C:\Users\josep\Documents\nixos-config\scripts\test-all-ssh.ps1
```

## Authentication
- **Primary**: SSH key authentication using unified key
- **Fallback**: Password authentication (enabled on most systems)
- **Synology Exception**: Auto Block protection instead of disabled password auth

## System Versions
- **NixOS Laptops**: NixOS 25.05 with latest updates
- **All LXC Containers**: Debian 13 (Trixie) with Python 3.13
- **Synology NAS**: DSM 7.x with Auto Block security
- **Proxmox Host**: Latest Proxmox VE

## Documentation Location
All infrastructure documentation moved to: `C:\Users\josep\Documents\docs\`

## Backup
Store the unified key in Bitwarden SSH vault for recovery.