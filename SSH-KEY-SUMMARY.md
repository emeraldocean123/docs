# SSH Key Configuration Summary

## Current GitHub SSH Keys
1. **Windows 11** - `SHA256:6BRj6x/t6rwZIAqm685j8g299u7BJ0uOEntbgmPs7NY`
   - Location: `~/.ssh/id_ed25519_github`
   - Added: Aug 7, 2025
   - Status: Active, used for git operations from Windows

2. **hp-dv9500-pavilion-nixos** - `SHA256:KIvefnb3+KGmS6w80QqXh6HQ4o0cTUJQWIO1bo9fTC0`
   - Location: HP laptop `~/.ssh/id_ed25519`
   - Added: Aug 9, 2025
   - Status: Active, used for git operations from HP laptop

## New Unified Key
- **Fingerprint**: `SHA256:2Oa0aH7dI5P+5H7wHTn5gsKWfHUw2oM5POWsBgP7AH8`
- **Location**: `~/.ssh/id_ed25519_unified`
- **Purpose**: Unified SSH access to all NixOS systems

## Deployment Status
- ✅ MSI laptop (192.168.1.106) - Has unified key in authorized_keys
- ✅ HP laptop (192.168.1.104) - Has unified key in authorized_keys
- ✅ Proxmox Host (192.168.1.40) - Has unified key in authorized_keys
- ✅ All LXC Containers - Have unified key in authorized_keys
- ✅ Synology NAS (192.168.1.10) - Has unified key with Auto Block protection
- ✅ GitHub - Unified key deployed and working

## Current Configuration (Implemented)

### ✅ Full Consolidation Complete
- **Unified key** used for everything: GitHub, SSH to all systems
- **Old GitHub keys** replaced with unified key
- **All systems** use the same unified SSH key
- **Simplified management** with one key for everything

## Current SSH Access
```bash
# SSH shortcuts (all use unified key automatically)
ssh nas         # Synology NAS
ssh hp          # HP laptop  
ssh msi         # MSI laptop
ssh proxmox     # Proxmox host
ssh wireguard   # WireGuard LXC
ssh tailscale   # Tailscale LXC
ssh omada       # Omada LXC
ssh netbox      # NetBox LXC
ssh iventoy     # iVentoy LXC
ssh docker      # Docker LXC
ssh syncthing   # Syncthing LXC

# GitHub operations (uses unified key)
git clone git@github.com:emeraldocean123/repo.git
```

## Bitwarden Backup
Store the unified key in Bitwarden for recovery:
- Private key: `~/.ssh/id_ed25519_unified`
- Public key: `~/.ssh/id_ed25519_unified.pub`