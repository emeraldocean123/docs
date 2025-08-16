# NixOS Configuration Best Practices

## Critical Syntax Requirements

### udev Rules
**NEVER use inline comments in udev rules within NixOS configurations.**

❌ **Wrong - Causes build failures:**
```nix
services.udev.extraRules = ''
  SUBSYSTEM=="usb", ATTRS{idVendor}=="046d", MODE="0666"  # Logitech
  SUBSYSTEM=="usb", ATTRS{idVendor}=="1532", MODE="0666"  # Razer
'';
```

✅ **Correct - Comments on separate lines:**
```nix
services.udev.extraRules = ''
  # Logitech devices
  SUBSYSTEM=="usb", ATTRS{idVendor}=="046d", MODE="0666"
  # Razer devices
  SUBSYSTEM=="usb", ATTRS{idVendor}=="1532", MODE="0666"
'';
```

**Error message:** `Invalid key/value pair, ignoring` and `udev rules check failed`

### Deprecated Options to Avoid

#### PulseAudio Configuration
❌ **Deprecated:**
```nix
hardware.pulseaudio.enable = false;
```

✅ **Current:**
```nix
services.pulseaudio.enable = false;
```

**Error message:** `The option 'hardware.pulseaudio' has been renamed to 'services.pulseaudio'`

## Configuration Structure Best Practices

### Modular Architecture
Our current structure (80% code reduction achieved):

```
modules/
├── shared/          # Base configurations used by all hosts
├── profiles/        # Desktop environments and hardware profiles
├── roles/           # Functional roles (gaming, multimedia, development)
└── hosts/           # Host-specific configurations
```

### File Organization Rules

1. **shared/**: Common configurations (users, base packages, services)
2. **profiles/**: Hardware types (laptop-base, gaming-hardware) and DE (kde-plasma, lxqt)
3. **roles/**: Functionality groups (gaming, multimedia, development, gaming-performance)
4. **hosts/**: Import appropriate profiles and roles for each machine

### Testing and Deployment Workflow

**MANDATORY WORKFLOW - NO EXCEPTIONS:**

1. **Always edit locally ONLY** in Windows `~/Documents/dev/nixos-config`
2. **Test syntax** with `nix flake check` if possible
3. **Commit and push** to GitHub
4. **Pull to target machine** before rebuilding
5. **NEVER edit directly** on NixOS laptops via SSH or local terminal

**This workflow is REQUIRED for all NixOS configuration changes.**

### Common Build Issues

#### Syntax Validation
- udev rules are strictly validated during build
- Inline comments break udev rule parsing
- Use separate comment lines instead

#### Option Deprecation
- NixOS regularly deprecates and renames options
- Always check deprecation warnings during builds
- Update deprecated options immediately to prevent future failures

#### Flake Configuration
- Host configurations must match flake output names exactly
- Example: `#msi-ge75-raider-nixos` must match flake.nix output

## Build Commands

### Standard Rebuild
```bash
sudo nixos-rebuild switch --flake .#<hostname>
```

### Dry Run (Test Only)
```bash
sudo nixos-rebuild dry-build --flake .#<hostname>
```

### Check Configuration
```bash
nix flake check
```

## Recovery Procedures

### If Build Fails
1. Check error message for specific syntax issues
2. Fix locally in Windows Documents/dev/nixos-config
3. Push to GitHub and pull to target machine
4. Retry rebuild

### If System Won't Boot
1. Boot from previous generation in GRUB
2. Fix configuration and rebuild
3. Previous generations are automatically preserved

## Home Manager Integration

- Configuration location: `~/nixos-config/` (not `/etc/nixos`)
- Uses flakes for declarative package management
- Manages both system and user environments
- Enables dotfiles synchronization across machines

## Notes for Claude Code Assistant

⚠️ **CRITICAL WORKFLOW REQUIREMENT** ⚠️

**ALWAYS follow this exact sequence - NO SHORTCUTS:**
1. Edit locally in Windows `~/Documents/dev/nixos-config` 
2. Commit and push to GitHub
3. Pull to target NixOS machine
4. Rebuild on target machine

**NEVER EVER:**
- Edit files directly on NixOS machines via SSH
- Use nano/vim/edit commands on target machines
- Make any changes outside the Windows local repository

**Additional Rules:**
- **Check for deprecated options** in build output
- **Remember udev rules cannot have inline comments**
- **Use the modular structure** when adding new configurations
- **Test with dry-build when possible**