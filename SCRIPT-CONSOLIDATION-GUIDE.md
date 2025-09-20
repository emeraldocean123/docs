# Script and Documentation Consolidation Guide

A comprehensive guide for regularly consolidating, cleaning up, and generalizing scripts and documentation across repositories.

## Overview

This guide documents the process for identifying and eliminating redundancy across multiple repositories, consolidating scripts into reusable utilities, and maintaining clean documentation.

## Consolidation Process

### Phase 1: Analysis and Discovery

#### 1.1 Find All Scripts and Documentation
```bash
# Find all scripts and documentation files
find Documents/ -name "*.sh" -o -name "*.ps1" -o -name "*.py" -o -name "*.md" | sort

# Or using PowerShell Glob
Documents/**/*.{sh,ps1,py,md}
```

#### 1.2 Identify Patterns
Look for:
- **Duplicate functionality**: Multiple scripts doing the same thing
- **Similar names**: `test-ssh.ps1`, `ssh-quick-test.ps1`, `test-all-ssh.ps1`
- **Scattered utilities**: SSH scripts in nixos-config instead of dotfiles
- **Redundant profiles**: Multiple PowerShell profiles in different locations
- **Outdated references**: Scripts referencing obsolete systems (e.g., CachyOS)

### Phase 2: Consolidation Strategy

#### 2.1 PowerShell Profile Consolidation

**Common Redundancies:**
- `Documents/PowerShell/Microsoft.PowerShell_profile.ps1`
- `Documents/PowerShell/Microsoft.VSCode_profile.ps1` 
- `Documents/dev/dotfiles/powershell/Microsoft.PowerShell_profile.ps1`
- Multiple `profile.bootstrap.ps1` files

**Consolidation Approach:**
1. Keep dotfiles version as single source of truth
2. Remove standalone copies in Documents/PowerShell
3. VSCode uses the same profile as PowerShell (no need for separate)
4. One bootstrap file for PSReadLine loading

#### 2.2 SSH Script Consolidation

**Before:** Individual connection scripts
```
nixos-config/scripts/connect-nixos.ps1
nixos-config/scripts/connect-cachyos.ps1
nixos-config/scripts/test-all-ssh.ps1
nixos-config/scripts/ssh-quick-test.ps1
```

**After:** Generalized utilities in shared/scripts
```
shared/scripts/ssh/test-ssh-connectivity.ps1  # All testing functionality
shared/scripts/ssh/connect-host.ps1           # All connection functionality
```

#### 2.3 Verification Script Consolidation

**Before:** Multiple verification scripts
```
dotfiles/powershell/Verify-Profile.ps1
shared/scripts/validation/validate-environment.ps1
shared/scripts/validation/validate-environment.ps1
```

**After:** Single comprehensive validator
```
shared/scripts/validation/validate-environment.ps1
```

### Phase 3: Implementation

#### 3.1 Create Consolidated Scripts

**Example: Unified SSH Connectivity Tester**
```powershell
# Combines functionality from multiple scripts
param(
    [string]$Target = "",
    [switch]$All,          # From test-all-ssh.ps1
    [switch]$Quiet,        # New feature
    [int]$TimeoutSec = 5   # From ssh-quick-test.ps1
)

# Infrastructure hosts from CLAUDE.md
$InfrastructureHosts = @(
    @{Name="GitHub"; Host="git@github.com"; Special="github"},
    @{Name="HP Laptop"; Host="joseph@hp"; IP="192.168.1.104"},
    # ... etc
)
```

**Example: Generalized Host Connector**
```powershell
# Single script replacing multiple connect-*.ps1 files
param(
    [Parameter(Position=0, Mandatory=$true)]
    [string]$HostAlias,    # hp, msi, nas, proxmox, etc.
    [string]$Command = "",
    [switch]$Interactive,
    [switch]$Info
)

$HostMap = @{
    "hp" = @{Target="joseph@hp"; IP="192.168.1.104"; Name="HP Laptop (NixOS)"}
    "msi" = @{Target="joseph@msi"; IP="192.168.1.106"; Name="MSI Laptop (NixOS)"}
    # ... etc
}
```

#### 3.2 Remove Redundant Files
```bash
# Remove duplicate PowerShell profiles
rm Documents/PowerShell/Microsoft.VSCode_profile.ps1
rm Documents/PowerShell/profile.bootstrap.ps1
rm Documents/PowerShell/Microsoft.PowerShell_profile.ps1

# Remove old verification scripts
rm Documents/dev/dotfiles/powershell/Verify-Profile.ps1
rm Documents/dev/shared/scripts/validation/validate-environment.ps1
rm Documents/dev/shared/scripts/validation/validate-environment.ps1

# Remove individual SSH scripts
rm Documents/dev/nixos-config/scripts/connect-nixos.ps1
rm Documents/dev/nixos-config/scripts/test-all-ssh.ps1
# ... etc
```

#### 3.3 Update References

**Update README files:**
```markdown
### Before
- After bootstrap, run `powershell/Verify-Profile.ps1`

### After  
- After bootstrap, run validation:
  - PowerShell: `shared/scripts/validation/validate-environment.ps1 -PowerShell`
  - Everything: `shared/scripts/validation/validate-environment.ps1 -All`
```

**Add cross-references between repos:**
```markdown
## Cross-Platform Utilities

SSH and connectivity utilities have been moved to the [dotfiles repository](https://github.com/emeraldocean123/dotfiles):
- **Test SSH connectivity**: `~/Documents/dev/shared/scripts/ssh/test-ssh-connectivity.ps1 -All`
- **Connect to hosts**: `~/Documents/dev/shared/scripts/ssh/connect-host.ps1 hp`
```

### Phase 4: Documentation

#### 4.1 Create Usage Documentation

**For consolidated utilities:**
```markdown
# SSH Utilities

## Scripts

### test-ssh-connectivity.ps1
Test SSH connectivity to infrastructure hosts.

**Usage:**
- Test all: `.\test-ssh-connectivity.ps1 -All`
- Test specific: `.\test-ssh-connectivity.ps1 -Target "user@host"`

### connect-host.ps1
Connect to infrastructure hosts using aliases.

**Available aliases:**
- `hp` - HP Laptop (192.168.1.104)
- `msi` - MSI Laptop (192.168.1.106)
# ... etc
```

## Regular Maintenance Schedule

### Weekly
- Review for new duplicate scripts
- Check for outdated references
- Verify consolidated scripts still work

### Monthly  
- Full consolidation review
- Update documentation
- Test all consolidated utilities

### Quarterly
- Major reorganization if needed
- Archive obsolete functionality
- Update this guide with new patterns

## Consolidation Principles

1. **Single Source of Truth**: One authoritative location for each utility
2. **Cross-Platform First**: Put utilities in dotfiles when possible
3. **Generalize Over Specialize**: One flexible script > multiple specific scripts
4. **Document Everything**: Clear usage guides and examples
5. **Test Before Removal**: Ensure consolidated version works before deleting originals

## Common Patterns to Watch For

### Red Flags for Consolidation
- Scripts with similar names (`test-*`, `check-*`, `verify-*`)
- Multiple scripts in different repos doing the same thing
- Scripts with hardcoded values that could be parameterized
- Separate scripts for each host/system
- Duplicate profile or configuration files

### Good Consolidation Candidates
- Connection/SSH scripts → Single connection manager
- Verification/validation scripts → Single validator with modes
- Host-specific scripts → Parameterized script with host map
- Profile files → Single profile with conditional logic

## Testing Consolidated Scripts

### Basic Validation
```powershell
# Test environment validator
shared/scripts/validation/validate-environment.ps1 -PowerShell
shared/scripts/validation/validate-environment.ps1 -All

# Test SSH utilities
shared/scripts/ssh/test-ssh-connectivity.ps1 -Target "git@github.com"
shared/scripts/ssh/connect-host.ps1 hp -Info
```

### Regression Testing
Before removing originals, ensure:
1. All functionality is preserved
2. New scripts handle edge cases
3. Error messages are helpful
4. Performance is acceptable

## Tracking Changes

### Commit Message Format
```
feat: Consolidate SSH utilities into dotfiles

- Merged 5 individual connection scripts into connect-host.ps1
- Combined 3 test scripts into test-ssh-connectivity.ps1
- Removed 11 redundant files
- Updated documentation references
```

### Change Log Entry
```markdown
## [Date] - Script Consolidation

### Added
- `shared/scripts/validation/validate-environment.ps1` - Unified validation
- `shared/scripts/ssh/` - Consolidated SSH utilities

### Removed
- 11 redundant scripts across repositories
- Duplicate PowerShell profiles

### Changed
- Updated all documentation to reference new utilities
```

## Recovery Plan

If consolidation causes issues:

1. **Backup location**: `~/backup-YYYYMMDD-HHMMSS/`
2. **Git history**: Can revert commits if needed
3. **GitHub**: Can restore from remote repository

## Success Metrics

- **File count reduction**: Target 30-40% fewer scripts
- **Functionality preserved**: 100% of features maintained
- **Documentation improved**: All utilities have clear usage guides
- **Cross-platform support**: Utilities work on Windows and Linux
- **Maintenance burden**: Reduced by having fewer files to update

## Next Steps

1. Schedule regular consolidation reviews
2. Create automated tests for consolidated utilities
3. Consider creating a "scripts" repository for truly universal utilities
4. Document any new patterns discovered during consolidation

---

*Last consolidation: 2025-01-16*
*Files removed: 11*
*New utilities created: 4*
*Reduction: ~40%*


