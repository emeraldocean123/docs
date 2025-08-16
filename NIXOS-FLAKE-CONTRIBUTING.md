# Contributing to NixOS Flake Configurations

## Overview

Guidelines for contributing to NixOS flake-based configurations with local checks for code quality and consistency.

## Development Environment

### Setting Up

```bash
# Enter development shell (provides formatting and linting tools)
nix develop

# Or use direnv for automatic shell activation
echo "use flake" > .envrc
direnv allow
```

### Available Tools

- **nixpkgs-fmt**: Nix code formatter
- **statix**: Static analysis for Nix
- **deadnix**: Find dead code in Nix files
- **nix flake check**: Validate flake configuration

## Code Quality Standards

### Formatting

All Nix files must be formatted with nixpkgs-fmt:

```bash
# Format all files
nix fmt

# Or manually
nixpkgs-fmt .
```

### Linting

Run static analysis before committing:

```bash
# Check for issues
nix run nixpkgs#statix -- check

# Auto-fix issues (review changes before committing)
nix run nixpkgs#statix -- fix

# Find unused code
nix run nixpkgs#deadnix -- --fail
```

### Pre-commit Checks

Run all checks before pushing:

```bash
# Format code
nix fmt

# Run static analysis
nix run nixpkgs#statix -- check

# Validate flake
nix flake check
```

## Git Workflow

### Commit Messages

Follow conventional commit format:

```
<type>(<scope>): <subject>

<body>

<footer>
```

Types:
- **feat**: New feature
- **fix**: Bug fix
- **docs**: Documentation changes
- **style**: Formatting, missing semicolons, etc.
- **refactor**: Code restructuring
- **test**: Adding tests
- **chore**: Maintenance tasks

Examples:
```
feat(desktop): add wayland support for KDE Plasma

fix(nvidia): resolve driver conflict with kernel 6.5

docs(readme): update installation instructions

refactor(modules): split services into separate files
```

### Git Hooks (Optional)

Enable pre-commit hooks for automatic checks:

```bash
# Create hooks directory
mkdir -p .githooks

# Create pre-commit hook
cat > .githooks/pre-commit << 'EOF'
#!/usr/bin/env bash
set -e

echo "Running pre-commit checks..."

# Format check
if command -v nixpkgs-fmt &> /dev/null; then
    echo "Checking Nix formatting..."
    nixpkgs-fmt --check . || {
        echo "Format errors found. Run 'nix fmt' to fix."
        exit 1
    }
fi

# Static analysis
if command -v statix &> /dev/null; then
    echo "Running static analysis..."
    statix check || {
        echo "Static analysis failed. Run 'statix fix' to fix issues."
        exit 1
    }
fi

echo "Pre-commit checks passed!"
EOF

chmod +x .githooks/pre-commit

# Configure git to use hooks
git config core.hooksPath .githooks
```

## File Organization

### Directory Structure

```
nixos-config/
├── flake.nix                 # Main flake definition
├── flake.lock                # Locked dependencies
├── hosts/                    # Host-specific configurations
│   └── hostname/
│       ├── configuration.nix # System configuration
│       └── hardware-configuration.nix
├── modules/                  # Reusable modules
│   ├── core/                # Core system modules
│   ├── desktop/             # Desktop environments
│   ├── services/            # System services
│   └── hardware/            # Hardware-specific modules
├── profiles/                 # Configuration profiles
│   ├── desktop.nix          # Desktop profile
│   ├── server.nix           # Server profile
│   └── laptop.nix           # Laptop profile
├── home/                     # Home Manager configurations
│   └── username/
│       └── home.nix
└── lib/                      # Helper functions
```

### Module Guidelines

Each module should:

1. Have a clear, single purpose
2. Include a descriptive header comment
3. Use appropriate option types
4. Provide defaults where sensible
5. Document complex logic

Example module structure:

```nix
# Desktop environment configuration (KDE Plasma 6)
# Provides Wayland-based KDE desktop with common applications

{ config, lib, pkgs, ... }:

let
  cfg = config.modules.desktop.plasma;
in
{
  options.modules.desktop.plasma = {
    enable = lib.mkEnableOption "KDE Plasma desktop";
    
    wayland = lib.mkOption {
      type = lib.types.bool;
      default = true;
      description = "Enable Wayland session";
    };
  };

  config = lib.mkIf cfg.enable {
    services.xserver = {
      enable = true;
      displayManager.sddm.enable = true;
      desktopManager.plasma6.enable = true;
    };
    
    # Additional configuration...
  };
}
```

## Testing Changes

### Local Testing

```bash
# Dry run (no changes applied)
sudo nixos-rebuild dry-build --flake .#hostname

# Test configuration (temporary, reverts on reboot)
sudo nixos-rebuild test --flake .#hostname

# Apply configuration
sudo nixos-rebuild switch --flake .#hostname
```

### Virtual Machine Testing

Test configurations in a VM before deploying:

```bash
# Build VM image
nix build .#nixosConfigurations.hostname.config.system.build.vm

# Run VM
./result/bin/run-hostname-vm
```

## Documentation Standards

### Inline Comments

- Use comments to explain **why**, not **what**
- Document complex logic and workarounds
- Reference issue numbers for bug workarounds

### Module Documentation

Each module should include:

```nix
{
  options.myModule = {
    enable = lib.mkOption {
      type = lib.types.bool;
      default = false;
      description = lib.mdDoc ''
        Enable the my module service.
        
        This module provides:
        - Feature A
        - Feature B
        - Integration with C
      '';
      example = true;
    };
  };
}
```

## Pull Request Checklist

Before submitting a PR:

- [ ] Code is formatted with `nix fmt`
- [ ] Static analysis passes (`statix check`)
- [ ] Flake check passes (`nix flake check`)
- [ ] Changes tested locally
- [ ] Commit messages follow convention
- [ ] Documentation updated if needed
- [ ] No secrets or sensitive data included

## Common Patterns

### Option Types

```nix
# Boolean
lib.mkOption {
  type = lib.types.bool;
  default = false;
}

# String
lib.mkOption {
  type = lib.types.str;
  default = "value";
}

# List
lib.mkOption {
  type = lib.types.listOf lib.types.package;
  default = [];
}

# Attribute set
lib.mkOption {
  type = lib.types.attrsOf lib.types.str;
  default = {};
}

# Enum
lib.mkOption {
  type = lib.types.enum [ "option1" "option2" ];
  default = "option1";
}
```

### Conditional Configuration

```nix
# Simple condition
config = lib.mkIf config.services.myService.enable {
  # Configuration when enabled
};

# Multiple conditions
config = lib.mkIf (config.services.myService.enable && config.networking.networkmanager.enable) {
  # Configuration when both conditions are true
};

# Merge multiple conditions
config = lib.mkMerge [
  (lib.mkIf condition1 { /* config 1 */ })
  (lib.mkIf condition2 { /* config 2 */ })
];
```

## Resources

- [Nix Pills](https://nixos.org/guides/nix-pills/) - Learn Nix concepts
- [NixOS Manual](https://nixos.org/manual/nixos/stable/)
- [Nix Flakes](https://nixos.wiki/wiki/Flakes)
- [nixpkgs Manual](https://nixos.org/manual/nixpkgs/stable/)
- [Home Manager Manual](https://nix-community.github.io/home-manager/)