---
title: My Journey as a First-Time Nixpkgs Contributor
description: Reflecting on my initial experience as a contributor to the Nixpkgs repository
slug: nixpkgs-first-timer
date: 2023-09-26 00:00:00+0000
image: 
categories:
    - Nix
tags:
    - Nix
    - nixpkgs
    - GitHub
    - Contribution
    - Pull Request
weight: 1
---

For a considerable time, I had aspired to become a package maintainer for a Linux distribution. Encouraged by my recent and positive encounters with NixOS and its community, I decided to take the plunge by maintaining a Nix package. In this post, I aim to share the valuable lessons I learned during my first package contribution. However, it's essential to note that for a comprehensive set of guidelines, one should always refer to the [CONTRIBUTING.md](https://github.com/NixOS/nixpkgs/blob/master/CONTRIBUTING.md) document. The content of this post may appear basic to some, and most of these insights are drawn from my experience with [this Pull Request](https://github.com/NixOS/nixpkgs/pull/257357) and the CONTRIBUTING.md guidelines.

## Package

### Naming

If the package you're adding has a name that conflicts with an existing package in nixpkgs, endeavor to give your package a meaningful and distinct name. In my case, the package I attempted to add was named "slurm," which had a naming conflict with an existing package. As suggested in my Pull Request, I ultimately named the package "slurm-nm" to clarify that it is a network monitoring tool.

### Placement

The package should be located within the new hierarchical structure, following the path `pkgs/by-name/sl/slurm-cli/package.nix`. There's no longer a need to include it at the top-level for it to function correctly.

### Build

Once the package is correctly placed, build it from the root of the project using `nix-build -A package-name`, and then verify that the package located at `result/bin/package` functions as expected.

### Contents

Ensure that the package does not contain any trailing whitespace, and there should be at most one newline between statements. If the `buildPhase` and `installPhase` do not perform any actions, you can exclude them from the derivation file. Additionally, the `meta` section in the file should provide information about the license, maintainer, and, in this case, the main program for the derivation. During this process, I learned that `license.gpl2` is deprecated in favor of `license.gpl2Plus` and `license.gpl2Only`.

## Commit Rules

### Adding Yourself as a Maintainer

If you have never committed anything to nixpkgs before, you should add yourself as a maintainer to the [maintainer-list.nix](https://github.com/NixOS/nixpkgs/blob/master/maintainers/maintainer-list.nix) file following the structure below:

```nix
    handle = {
      # Required
      name = "Your name";

      # Optional, but at least one of email, matrix, or githubId must be given
      email = "address@example.org";
      matrix = "@user:example.org";
      github = "GithubUsername";
      githubId = your-github-id;

      keys = [{
        fingerprint = "AAAA BBBB CCCC DDDD EEEE  FFFF 0000 1111 2222 3333";
      }];
    };
```

This addition to the maintainer-list must be in alphabetical order. The commit adding yourself to this list should adhere to the guidelines and, therefore, have a commit message like "maintainers: add mikaelfangel". Importantly, this commit MUST precede a commit that adds the package. If the commit ordering is incorrect, use `git rebase -i` to reorder the commits. Once done, use `git push --force-with-lease` to push the changes.

### Adding the Package

When adding the package, the commit message should follow the format "package-name: init at version". For example, "slurm: init at 0.4.4".
