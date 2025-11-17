---
date: 2025-11-17
draft: false
slug: nixos-etc-symlink-issue-nix-shell
comments: true
categories:
  - NixOS
tags:
  - nix-shell
  - home-manager
  - troubleshooting
description: Why /etc/nixos can cause symlink issues in nix-shell and how moving your config to a user directory solves the problem
---

# A Weird Reason Not to Store your Nix Config in /etc/nixos

A quick one today.

If you're new to NixOS and use `home-manager`, you might run into a subtle issue where your program configs mysteriously break inside nix shells.

<!-- more -->

## The Problem

When you open a nix shell:

=== "With Flakes"
    ```bash
    nix develop
    ```

=== "Without Flakes"
    ```bash
    nix-shell
    ```

You might suddenly find that configs in your programs don't work.

## What's Happening

`home-manager` symlinks your config files through the nix store to locations in your NixOS configuration directory (by default `/etc/nixos`).

For example:
```bash
~/.config/nvim -> /nix/store/...-home-manager-files/.config/nvim -> /etc/nixos/home-manager/nvim
```

However, when you enter a nix shell, the `/etc` directory is remapped to a temporary one that only contains essentials from the nix store. Your `/etc/nixos` is not included, so these symlinks break.

## The Solution

Move your NixOS configuration to a user directory like `~/nixos`:

```bash
sudo mv /etc/nixos ~/nixos
```

Then rebuild from the new location:

=== "With Flakes"
    ```bash
    sudo nixos-rebuild switch --flake ~/nixos#hostname
    ```

=== "Without Flakes"
    ```bash
    sudo nixos-rebuild switch -I nixos-config=~/nixos/configuration.nix
    ```

!!! tip "TL;DR"
    Don't store your NixOS config in `/etc/nixos` if you use home-manager and nix-shell. Move it to a user directory like `~/nixos` to avoid broken symlinks.

---

*For the curious: I started using NixOS in July 2024 and ran into this issue around September.*