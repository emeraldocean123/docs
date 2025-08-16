# NixOS Installation Guide

## Overview

This guide covers installing NixOS with flakes and Home Manager for a declarative, reproducible system configuration.

## Prerequisites

- NixOS 25.05 ISO (or latest stable)
- UEFI-enabled system (disable Secure Boot for proprietary drivers)
- Internet connection during installation
- Backup of any existing data

## Installation Process

### 1. Boot and Partition

Boot from NixOS installer USB and partition your disk:

```bash
# Identify your disk
lsblk

# Partition (example for /dev/nvme0n1)
parted /dev/nvme0n1 -- mklabel gpt
parted /dev/nvme0n1 -- mkpart ESP fat32 1MB 512MB
parted /dev/nvme0n1 -- mkpart primary ext4 512MB 100%
parted /dev/nvme0n1 -- set 1 esp on

# Format partitions
mkfs.fat -F32 /dev/nvme0n1p1
mkfs.ext4 /dev/nvme0n1p2

# Mount filesystems
mount /dev/nvme0n1p2 /mnt
mkdir -p /mnt/boot
mount /dev/nvme0n1p1 /mnt/boot
```

### 2. Generate Hardware Configuration

```bash
# Generate config based on detected hardware
nixos-generate-config --root /mnt

# This creates:
# - /mnt/etc/nixos/configuration.nix
# - /mnt/etc/nixos/hardware-configuration.nix
```

**⚠️ CRITICAL**: The `hardware-configuration.nix` file contains machine-specific UUIDs that are UNIQUE to each installation. Never copy this file between machines!

### 3. Enable Flakes

Edit `/mnt/etc/nixos/configuration.nix` to enable flakes:

```nix
{
  nix.settings.experimental-features = [ "nix-command" "flakes" ];
  
  # Set hostname
  networking.hostName = "your-hostname";
  
  # Enable NetworkManager
  networking.networkmanager.enable = true;
  
  # Enable SSH (optional)
  services.openssh.enable = true;
}
```

### 4. Install NixOS

```bash
nixos-install
# Set root password when prompted
reboot
```

### 5. Post-Installation Setup

After first boot, log in as root:

```bash
# Apply flakes support
nixos-rebuild switch

# Create user account (if not done during install)
useradd -m -G wheel,networkmanager -s /bin/bash username
passwd username
```

## Using Flake-Based Configuration

### Setting Up Your Configuration Repository

```bash
# Clone your configuration
cd ~
git clone https://github.com/yourusername/nixos-config.git

# IMPORTANT: Preserve YOUR hardware configuration
sudo cp /etc/nixos/hardware-configuration.nix ~/nixos-config/hosts/your-host/

# Switch to flake configuration
cd ~/nixos-config
sudo nixos-rebuild switch --flake .#your-hostname
```

### Configuration Structure

Recommended flake repository structure:

```
nixos-config/
├── flake.nix              # Main flake file
├── flake.lock             # Pinned dependencies
├── hosts/
│   └── hostname/
│       ├── configuration.nix
│       └── hardware-configuration.nix
├── modules/               # Shared modules
│   ├── desktop.nix
│   ├── development.nix
│   └── services.nix
└── home/                  # Home Manager configs
    └── username.nix
```

## Hardware-Specific Considerations

### NVIDIA GPUs

For systems with NVIDIA GPUs:

```nix
{
  # Enable proprietary NVIDIA drivers
  services.xserver.videoDrivers = [ "nvidia" ];
  
  # For newer cards
  hardware.nvidia = {
    modesetting.enable = true;
    open = false;  # Use proprietary drivers
    nvidiaSettings = true;
    package = config.boot.kernelPackages.nvidiaPackages.stable;
  };
}
```

### Laptop-Specific Settings

```nix
{
  # Power management
  services.power-profiles-daemon.enable = true;
  
  # Lid behavior
  services.logind = {
    lidSwitch = "suspend";
    lidSwitchExternalPower = "lock";
  };
  
  # Battery optimization
  services.tlp.enable = true;
}
```

## Recovery and Troubleshooting

### Boot Issues

If system won't boot after configuration changes:

1. Boot from installer USB
2. Mount your partitions:
   ```bash
   mount /dev/nvme0n1p2 /mnt
   mount /dev/nvme0n1p1 /mnt/boot
   ```
3. Regenerate hardware config:
   ```bash
   nixos-generate-config --root /mnt
   ```
4. Rebuild from chroot:
   ```bash
   nixos-enter --root /mnt -- nixos-rebuild boot
   ```

### Rollback to Previous Generation

```bash
# List generations
sudo nix-env --list-generations --profile /nix/var/nix/profiles/system

# Rollback
sudo nixos-rebuild switch --rollback

# Or boot specific generation
sudo nix-env --profile /nix/var/nix/profiles/system --switch-generation 42
```

### Verify Hardware Configuration

Always verify UUIDs match your actual hardware:

```bash
# Check disk UUIDs
lsblk -f
blkid

# Compare with hardware-configuration.nix
cat /etc/nixos/hardware-configuration.nix | grep UUID
```

## Best Practices

1. **Version Control**: Keep your configuration in Git
2. **Test Changes**: Use `nixos-rebuild test` before `switch`
3. **Pin Inputs**: Use `flake.lock` for reproducibility
4. **Modular Config**: Split configuration into logical modules
5. **Hardware Config**: Never share `hardware-configuration.nix` between machines
6. **Backup**: Keep USB installer handy for recovery

## Useful Commands

```bash
# Update flake inputs
nix flake update

# Check configuration
nixos-rebuild dry-build --flake .#hostname

# Apply configuration
sudo nixos-rebuild switch --flake .#hostname

# Garbage collection
sudo nix-collect-garbage -d

# Show system info
nixos-version --json
```

## Home Manager Integration

For user-specific configurations:

```nix
{
  home-manager = {
    useGlobalPkgs = true;
    useUserPackages = true;
    users.username = import ./home/username.nix;
  };
}
```

## Resources

- [NixOS Manual](https://nixos.org/manual/nixos/stable/)
- [Nix Flakes Documentation](https://nixos.wiki/wiki/Flakes)
- [Home Manager Manual](https://nix-community.github.io/home-manager/)
- [NixOS Options Search](https://search.nixos.org/options)