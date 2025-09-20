# Codex CLI Workspace Guide for ~/Documents/dev

This document mirrors the guidance in `CLAUDE.md` so both assistants operate with the same context, repos, and conventions.

## Repositories

1. `~/Documents/dev/bookmark-cleaner` -> bookmark tool
2. `~/Documents/dev/dotfiles` -> cross‑platform configs and shared githooks (+ VS Code workspace)
3. `~/Documents/dev/nixos-config` -> NixOS configs (+ development tools)
4. `~/Documents/dev/docs` -> documentation and guides (+ examples)
5. `~/Documents/dev/bmad-method` -> BMAD Method AI agent framework
6. `~/Documents/dev/maintenance` -> Windows system maintenance toolkit
7. `~/Documents/dev/bmad-agents` -> project-specific BMAD agents library

## Conventions

- SSH: unified key at `~/.ssh/id_ed25519_unified` (Windows: `C:\Users\josep\.ssh\id_ed25519_unified`).
- Git: remotes use SSH; global `core.hooksPath` → `Documents/dev/dotfiles/githooks`.
- Commits: Conventional Commits; repos use a `commit.template` (global fallback in dotfiles).
- Formatting: `.editorconfig` present; repos avoid CRLF issues; PowerShell `.ps1` stays CRLF.

## Tasks to Prefer

- Use repo scripts and shared hooks; avoid touching unrelated files.
- For multi‑repo work, update READMEs and keep CI green (link‑check, secret‑scan; Nix flake check for `nixos-config`).
- Validate PowerShell profiles under `Documents/PowerShell` and `Documents/WindowsPowerShell` remain in sync with `dotfiles/powershell/profile.bootstrap.ps1`.

## WSL + Nix

- Use Debian (WSL2) + Nix (not NixOS). See `Documents/dev/nixos-config/tools/wsl/FIRST-RUN.md` for a short checklist.
  - Bootstrap scripts:
    - `Documents/dev/nixos-config/tools/wsl/bootstrap-nix-debian.sh --write-wslconf` (sudo)
    - Windows PowerShell: `wsl --shutdown`, reopen Debian
    - `Documents/dev/nixos-config/tools/wsl/bootstrap-nix-debian.sh`

## Maintenance

- Weekly Windows cleanup task runs `Documents/dev/maintenance/scripts/cleanup-dev-repos.ps1` (safe junk removal).
- CI: link checks, secret scans across repos; Nix flake check in `nixos-config`.

## Cross‑Assistant Notes

- This file is kept aligned with `CLAUDE.md`. If you update one, reflect it in the other (repos, workflows, paths, SSH, conventions).




