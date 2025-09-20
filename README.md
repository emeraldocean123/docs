# Documentation Repository

This repository contains various technical documentation and guides for system administration, infrastructure setup, and development workflows.

## Contents

### System Administration
- **INFRASTRUCTURE.md** - Infrastructure setup and configuration
- **DEBIAN-13-UPGRADE-REPORT.md** - Debian 13 system upgrade report
- **NETWORK-DEVICES-COMPLETE.md** - Complete network device inventory

### SSH & Security
- **SSH-KEY-SUMMARY.md** - SSH key management summary
- **UNIFIED-KEY-SETUP.md** - Unified key setup documentation
- **SYNOLOGY-SSH-SETUP.md** - Synology NAS SSH configuration guide
- **OPNSENSE-SSH-SETUP.md** - OPNsense router SSH setup
- **PSREADLINE-SECURITY-MONITORING.md** - PowerShell security monitoring

### NixOS Documentation
- **NIXOS-INSTALLATION-GUIDE.md** - NixOS installation guide
- **NIXOS-CONFIG-BEST-PRACTICES.md** - NixOS configuration best practices
- **NIXOS-FLAKE-CONTRIBUTING.md** - Contributing to NixOS flakes
- **NIXOS-HARDWARE-CONFIG-WARNING.md** - Hardware configuration safety
- **HP-DUAL-BOOT-GUIDE.md** - HP laptop dual-boot setup

### Development & Maintenance
- **SCRIPT-CONSOLIDATION-GUIDE.md** - Guide for consolidating and cleaning up scripts
- **COPILOT-INTEGRATION-SUMMARY.md** - GitHub Copilot integration documentation

## Purpose

This repository serves as a centralized location for technical documentation, configuration guides, and system administration notes.

## License

Private repository - for personal use only
## Conventional Commits & Hooks

Documentation updates also follow Conventional Commits for consistency.

- Global Commit Template: configured to `Documents/dev/dotfiles/git-templates/commit_template.txt`.
- Shared Hooks: `core.hooksPath` → `Documents/dev/dotfiles/githooks`.
  - `pre-commit`: blocks obvious secrets and large files; use `.githooks-allow.txt` to allow known safe paths.
  - `commit-msg`: validates Conventional Commit format; bypass with `GITHOOKS_BYPASS=1` if necessary.

## CI Checks

- Link Check (lychee) and Secret Scan (gitleaks) run on pushes and PRs; scheduled weekly.
- Markdownlint runs for all Markdown files; see `.markdownlint.jsonc` for exceptions.

## Assistant Guides

- See `Documents/dev/CODEX.md` and `Documents/dev/CLAUDE.md` for the shared workspace context and conventions.

## Review Routing

- CODEOWNERS is set to `@emeraldocean123`; PRs route reviews automatically.

## Contributing

- Use Conventional Commits for all messages: `type(scope)?: subject`.
- Commit Template: this repo is configured with a commit message template to guide messages.
- Hooks: shared `pre-commit` and `commit-msg` hooks run automatically (configured via global `core.hooksPath`).
- Bypass (rare): set `GITHOOKS_BYPASS=1` to skip checks once.
## Shared Resources

All script references now point to the centralized helpers in `~/Documents/dev/shared`. Update that repository first to ensure the validation and SSH utilities referenced throughout this documentation are available.

