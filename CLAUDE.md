# Claude Assistant Context

## IMPORTANT RULES
- **GitHub Actions**: NEVER create .github directories, workflow files, or any CI/CD automation
- **All repositories explicitly prohibit GitHub Actions** - see .clinerules files in each repo
- This applies to Claude, Codex, Copilot, and all AI assistants

## Working Directories

These are my active git repositories and working directories:

1. **~/Documents/dev/bookmark-cleaner** - Bookmark cleaning and organization tool
2. **~/Documents/dev/dotfiles** - Personal dotfiles and configurations  
3. **~/Documents/dev/nixos-config** - NixOS system configuration (Windows copy)
   - **Note**: NixOS laptops use `~/nixos-config/` with Home Manager and flakes
4. **~/Documents/dev/docs** - Technical documentation and guides
5. **~/Documents/dev/bmad-method** - BMAD Method AI agent framework with custom NixOS expert

## Quick Navigation

When starting a session, you can quickly navigate to any project:
- `cd Documents/dev/bookmark-cleaner`
- `cd Documents/dev/dotfiles`
- `cd Documents/dev/nixos-config`
- `cd Documents/dev/docs`
- `cd Documents/dev/bmad-method`

## VS Code Workspace

**Workspace File**: `~/Documents/dev/vscode/joseph.code-workspace`

To open the complete workspace in VS Code:
```bash
code ~/Documents/dev/vscode/joseph.code-workspace
```

The workspace includes all working directories with:
- 📋 bookmark-cleaner - Python bookmark management tool
- 🔧 dotfiles - Cross-platform shell configurations  
- 🐧 nixos-config - NixOS system configurations
- 📚 docs - Technical documentation and guides
- 🤖 bmad-method - AI agent framework with custom NixOS expert

### VS Code Settings
- **Claude Code extension** integrated for AI assistance
- **Multi-repository setup** with consistent formatting
- **Language support** for Python, PowerShell, Nix, JSON, YAML
- **Git integration** with smart commit and sync
- **Auto-formatting** on save enabled

## Folder Organization

**Documents Folder Structure** (reorganized for better organization):
```
📁 Documents/
├── 🔧 dev/                          # Development workspace
│   ├── bookmark-cleaner/            # Python bookmark tool
│   ├── dotfiles/                    # Shell configurations
│   ├── docs/                        # Technical documentation
│   ├── nixos-config/                # NixOS configurations
│   ├── bmad-method/                 # AI agent framework
│   └── vscode/                      # VS Code workspace
├── 🛠️ utilities/                    # Portable apps & tools
│   ├── Advanced Renamer/
│   ├── Bulk Rename Utility/
│   ├── CrapFixer/
│   └── [... other utilities]
├── 📁 PowerShell/                   # PowerShell 7 profiles
├── 📁 WindowsPowerShell/            # PowerShell 5.1 profiles
└── 📁 [Other system folders]        # OBS, OnScreen Control, etc.
```

**Benefits:**
- ✅ Clean workspace - All development in organized `dev/` folder
- ✅ Easy navigation - Logical grouping by purpose  
- ✅ Portable apps contained - No more clutter at root level
- ✅ Preserves functionality - PowerShell profiles stay in correct locations

## Common Tasks

- Check all repositories status: Run `for dir in Documents/dev/{bookmark-cleaner,dotfiles,nixos-config,docs,bmad-method}; do echo "=== $dir ===" && cd $dir && git status && cd ~/; done`
- Pull all repositories: Run `for dir in Documents/dev/{bookmark-cleaner,dotfiles,nixos-config,docs,bmad-method}; do echo "=== $dir ===" && cd $dir && git pull && cd ~/; done`

## BMAD Method & AI Agents

### BMAD Method Framework
- **Location**: `~/Documents/dev/bmad-method/`
- **Purpose**: Universal AI Agent Framework for development workflows
- **Custom Extensions**: NixOS Expert with MCP integration
- **Status**: Ready to push to GitHub (repository needs to be created first)

### Organized Agent Library
- **Location**: `~/Documents/dev/bmad-agents/` (for organized access)
- **Structure**:
  - `bookmark-cleaner/` - Market analysis and product enhancement agents
  - `nixos-config/` - Configuration optimization and security review
  - `dotfiles/` - Development automation and cross-platform scripting
  - `docs/` - Documentation strategy and content organization
  - `general/` - General-purpose agents for various tasks

### Custom NixOS Expert Agent
- **Agent File**: `bmad-method/expansion-packs/bmad-nixos-expert/agents/nixos-expert.md`
- **Built Bundle**: `bmad-method/dist/expansion-packs/bmad-nixos-expert/agents/nixos-expert.txt`
- **Commands**: optimize-config, create-module, review-flake, hardware-config, home-manager-setup, troubleshoot-build, security-review, multi-machine
- **MCP Integration**: Real-time NixOS package and option validation via MCP-NixOS server

## Infrastructure

### SSH Configuration
- **Unified SSH Key**: `~/.ssh/id_ed25519_unified` (SHA256:2Oa0aH7dI5P+5H7wHTn5gsKWfHUw2oM5POWsBgP7AH8)
- **Authentication**: Always use SSH key authentication (password fallback available)

### Network Devices

#### Network Infrastructure
| Host | IP | SSH Shortcut | Description |
|------|-----|--------------|-------------|
| OPNsense Router | 192.168.1.1 | `ssh opnsense` | Router/Firewall VM (joseph/root users, FreeBSD 14.3) |
| TP-Link SG3428-2 | 192.168.1.2 | - | Managed switch (SSH disabled, use Omada) |
| Linksys MX4200 (Joe) | 192.168.1.5 | - | Mesh router node 1 |
| Linksys MX4200 (Kitchen) | 192.168.1.6 | - | Mesh router node 2 |
| Linksys MX4200 (Family) | 192.168.1.7 | - | Mesh router node 3 |
| Linksys MX4200 (Mark) | 192.168.1.8 | - | Mesh router node 4 |
| Linksys MX4200 (Gazebo) | 192.168.1.9 | - | Mesh router node 5 |

#### Core Systems (Static IPs)
| Host | IP | SSH Shortcut | Description |
|------|-----|--------------|-------------|
| Synology DS1520+ | 192.168.1.10 | `ssh nas` | NAS storage (Auto Block: 10 attempts/5 min) |
| Proxmox VE | 192.168.1.40 | `ssh proxmox` | Main virtualization host (root user) |
| HP Pavilion dv9500 | 192.168.1.104 | `ssh hp` | NixOS 25.05 + Home Manager + LXQt |
| MSI GE75 Raider | 192.168.1.106 | `ssh msi` | NixOS 25.05 + Home Manager + KDE Plasma 6 |

#### DHCP Devices
| Host | Current IP | Description |
|------|------------|-------------|
| Alienware 18 Area-51 | 192.168.1.197 | Windows 11 workstation (this machine) |

#### LXC Containers (Static IPs, Debian 13)
| Service | IP | SSH Shortcut | CTID | Purpose |
|---------|-----|--------------|------|---------|
| WireGuard | 192.168.1.50 | `ssh wireguard` | 1000 | VPN server |
| Tailscale | 192.168.1.51 | `ssh tailscale` | 1001 | Mesh VPN |
| Omada | 192.168.1.52 | `ssh omada` | 1002 | Network controller |
| NetBox | 192.168.1.53 | `ssh netbox` | 1003 | IPAM/DCIM |
| iVentoy | 192.168.1.54 | `ssh iventoy` | 1004 | PXE boot server |
| Docker | 192.168.1.55 | `ssh docker` | 1005 | Container services |
| Syncthing | 192.168.1.56 | `ssh syncthing` | 1006 | File synchronization |
| Umbrel | 192.168.1.60 | - | VM | Bitcoin node/personal server |

#### Workstations (Static IPs)
| Device | IP | Description |
|--------|-----|-------------|
| Dell Inspiron 3847 | 192.168.1.100 | Desktop PC |
| Dell Inspiron 3030 | 192.168.1.101 | Desktop PC |
| Windows 7 VM | 192.168.1.102 | Virtual Machine |

#### Other Static IP Devices (No SSH)
| Device | IP | Access Method | Description |
|--------|-----|---------------|-------------|
| Reolink NVR | 192.168.1.20 | http://192.168.1.20 | Security camera NVR |
| Epson ET-3830 | 192.168.1.21 | Web/App | Printer |

#### DHCP Devices (No SSH)
| Device | Current IP | Access Method | Description |
|--------|------------|---------------|-------------|
| Google Nest | ~192.168.1.107 | App only | Thermostat (IP may change) |
| Roomba | ~192.168.1.139 | App only | Vacuum robot (IP may change) |

### Quick Infrastructure Commands
```bash
# Check all SSH-enabled systems connectivity (ordered by IP)
for host in opnsense nas proxmox wireguard tailscale omada netbox iventoy docker syncthing hp msi; do
    echo -n "$host: "
    ssh -o ConnectTimeout=2 $host "hostname" 2>/dev/null || echo "offline"
done

# Manage LXC from Proxmox
ssh proxmox "pct list"              # List containers
ssh proxmox "pct start 1000"        # Start WireGuard
ssh proxmox "pct exec 1000 -- bash" # Execute in container
```

## System Information Display

### Fastfetch & MOTD Setup
All SSH-accessible systems now have dynamic system information displays:

**LXC Containers** (All 7 containers):
- **MOTD Banner**: `/etc/profile.d/00_lxc-details.sh` - Shows container name, community scripts attribution, Debian 13, hostname, and IP
- **Fastfetch**: Added to `/root/.bashrc` - Shows real-time system information
- **On-demand**: `info` command available - Clears screen and shows MOTD + fastfetch

**Proxmox Host**:
- **Fastfetch**: Added to `/root/.bashrc` - Shows Proxmox VE system information  
- **On-demand**: `info` command available - Clears screen and shows fastfetch

**Synology NAS**:
- **Neofetch**: Installed in `~/bin/neofetch` - Shows NAS system information
- **PATH**: Added `~/bin` to PATH in `.bashrc`

**OPNsense Router** (joseph user):
- **Custom Script**: `/home/joseph/bin/sysinfo.sh` - FreeBSD-compatible system info
- **Integration**: Added to `.profile` - Shows after OPNsense welcome banner
- **Root user**: Keeps original interactive menu unchanged

### Quick Commands
```bash
# On any LXC container or Proxmox host
info                    # Show MOTD + fastfetch with clear screen
fastfetch              # Show just fastfetch

# Test all system connectivity with info display (ordered by IP)
for host in proxmox wireguard tailscale omada netbox iventoy docker syncthing; do
    echo "=== $host ===" && ssh $host "info"
done
```

## Application Updates & Maintenance

### WireGuard & WGDashboard (192.168.1.50)
**WGDashboard Location**: `/etc/wgdashboard/src/`
**Current Version**: v4.2.5
**Service**: `wg-dashboard.service`

**Update Procedure**:
```bash
ssh wireguard "cd /etc/wgdashboard/src && systemctl stop wg-dashboard"
ssh wireguard "cd /etc/wgdashboard/src && echo 'Y' | ./wgd.sh update"
ssh wireguard "systemctl start wg-dashboard && systemctl status wg-dashboard"
```

**WireGuard Interface**: 
- Interface: wg0 on port 51820
- Network: 10.246.235.0/24
- Dashboard: http://192.168.1.50:10086

**System Updates**:
```bash
ssh wireguard "apt update && apt upgrade -y"  # Updates WireGuard tools + kernel
```

### Docker Containers (192.168.1.55)
**Location**: `/root/immich-app/` - Immich photo management
**Services**:
- Immich Server: `ghcr.io/immich-app/immich-server:release`
- Immich ML: `ghcr.io/immich-app/immich-machine-learning:release`  
- PostgreSQL: `tensorchord/pgvecto-rs:pg14-v0.2.0`
- Redis: `redis:6.2-alpine`
- Portainer: `portainer/portainer-ce:latest`

**Update Procedure**:
```bash
ssh docker "cd /root/immich-app && docker compose pull && docker compose up -d"
ssh docker "docker system prune -f"  # Clean old images
```

**Access**:
- Immich: http://192.168.1.55:2283
- Portainer: https://192.168.1.55:9443

## Notes

- All repositories use main branch
- GitHub username: emeraldocean123
- Git user: Joseph
- All LXC containers run Debian 13 (Trixie) with Python 3.13
- NixOS laptops run version 25.05 with Home Manager and flakes
  - Configuration location: `~/nixos-config/` (modular architecture)
  - Uses shared/profiles/roles structure for 80% code reduction
  - Declarative package management via flakes
  - **⚠️ MANDATORY WORKFLOW - NO EXCEPTIONS ⚠️**: 
    1. **ALWAYS edit locally ONLY** in `~/Documents/nixos-config` (Windows)
    2. Commit and push to GitHub
    3. Pull to target NixOS laptop  
    4. Run `sudo nixos-rebuild switch --flake .#hostname`
    5. **NEVER edit directly** on NixOS machines via SSH
  - **Critical syntax rules**:
    - NO inline comments in udev rules (use separate lines)
    - Use `services.pulseaudio` not `hardware.pulseaudio`
    - Check for deprecation warnings in build output
- All systems have `info` command for on-demand system information display
- **PowerShell Configuration**: Always use PowerShell 7 (`pwsh`) for full features
  - PowerShell 7: Complete Oh My Posh theme, PSReadLine 2.4.1, fastfetch, Git shortcuts
  - PowerShell 5.1: Minimal profile (basic Git shortcuts only) to avoid infinite loops
  - Claude Code configured to use PowerShell 7 via `SHELL=pwsh` environment variable