---
title: Demystifying Nix Flakes - Part 1
description: Delving into Nix Flakes - My First Experience with Streamlined Package Management in NixOS
slug: demystifying-nix-flakes-part1
date: 2023-09-28 00:00:00+0000
image: cover.jpg
categories:
    - Nix
tags:
    - Nix
    - NixOS
    - Nix Flakes
    - Tutorial
weight: 1
---

## Motivation

Nix Flakes introduce a new way of managing and deploying Nix packages and configurations. This post aims to provide an overview of what Nix Flakes are, how they differ from traditional Nix packages, and how to enable and use them on your system.

## What Are Nix Flakes?

Nix Flakes are a new feature in the Nix package manager that offer a more declarative and reproducible approach to package management and system configuration. They are designed to address some of the limitations of traditional Nix package management.

## Differences Between Flakes and Traditional Nix Packages

Nix Flakes differ from traditional Nix packages in several key ways:

- **Declarative Configuration**: Flakes encourage a more declarative configuration style, making it easier to specify precisely what packages and system configurations you want.

- **Reproducibility**: Flakes make it easier to create reproducible builds and system configurations, reducing the chances of unexpected changes.

- **Git Integration**: Flakes leverage Git for fetching dependencies, enabling more flexible and versioned package management.

- **Simplified Channel Management**: Flakes simplify channel management by allowing you to specify dependencies directly in your flake.nix file.

## Enabling Flakes on Your System

To enable Nix Flakes system-wide on NixOS, follow these steps:

1. Open your `configuration.nix` file.

2. Add the following lines to enable the `nix-command` and `flakes` features:

```nix
  nix.settings.experimental-features = [ "nix-command" "flakes" ];
```

3. Since Flakes use Git to pull dependencies, ensure that Git is installed by adding it to your system packages:

```nix
  environment.systemPackages = with pkgs; [ git ];
```

4. Apply the changes by running the following command:

```bash 
  sudo nixos-rebuild switch
```

You have now successfully enabled `nix flakes` and the new `nix command` on your system.

## Managing System Channels Using Nix Flakes

When flakes is enabled, it first look for a file called `/etc/nixos/flake.nix`. If not found, it will fallback to using `/etc/nixos/configuration.nix`.

To adapt your existing configuration for use with Flakes, you can use the following template. Be sure to adjust the system and architecture to match your setup:

```nix
{
  description = "My NixOS configuration";

  inputs = {
    nixpkgs = { url = "github:nixos/nixpkgs/nixos-23.05"; };
  };

  outputs = inputs:
  {
    nixosConfigurations = {

      nixos = inputs.nixpkgs.lib.nixosSystem {
        system = "x86_64-linux";
        specialArgs = { inherit inputs; };
        modules = [
          ./configuration.nix
        ];
      };
    };
  };
}

```

If you choose to manage your system using a flake.nix, you can remove your current channels by running:

```bash
sudo nix-channel --list
sudo nix-channel --remove nixos-23.05
```

Additionally, consider removing the home-manager channel if it has been added.

Here's an example of a configuration file that preserves your existing config and home-manager setup:

```nix
{
  description = "The flake configuration of my NixOS machine";

  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs/nixos-23.05";
    home-manager.url = "github:nix-community/home-manager/release-23.05";
    home-manager.inputs.nixpkgs.follows = "nixpkgs";
  };

  outputs = inputs@{nixpkgs, home-manager, ...}:
  {
    nixosConfigurations = {

      nixos = nixpkgs.lib.nixosSystem {
        system = "x86_64-linux";
        specialArgs = { inherit inputs; };
        modules = [
          ./configuration.nix
        ];
      };
    };
  };
}
```

With this configuration, you can effectively manage your system using Nix Flakes while preserving your previous setup.

## How to enable autoUpgrade with flakes?

Because flakes write a lock the old way of configuring autoUpgrade no longers works unless we pass some flags to it. To have autoUpgrade as before you could do the following:

```nix
  # Auto upgrade the system
  system.autoUpgrade = {
    enable = true;
    flake = inputs.self.outPath;
    flags = [ "--update-input" "nixpkgs" ]; 
    persistent = true;
    dates = "daily";
  };
```

 > Photo by [Marc Newberry](https://unsplash.com/@downrightpunch?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/omuw_oYkXs0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
