---
title: Cleaning Up Nix Residues
description: A short guide about managing residues in your NixOS setup!
slug: nix-cleanup
date: 2022-09-09 02:30:00+0000
---

# Introduction
Managing your NixOS system is not always a simple task. It sometimes requires
you to look in directions that you have not previously thought of.

One of such examples is my recent encounter with the residues left floating
around in both:
1. `/nix/var/nix/profiles/per-user/`
2. `/nix/var/nix/gcroots/per-user/`

Where both of these directories had retained my pre-flake setup (1 year old) and
never complained about the garbage even though I had on many occasions used:
1. `sudo nix-collect-garbage -d`
2. `sudo nix store gc`
3. `nix store optimise`

After venturing inside my `/nix/store` directory, I had found package builds
tracing back to `2021-08-26`, the date I used to run a non-flake based NixOS
configuration. That had me surprised and after continues questionings and
command pasting, I had came across the following command (output by Nix):
`nix store query --roots /nix/store/dapkosdkaspokdqw-old-package`; a command
intended to help you unravel what could be using a certain package.

Apparently, `nix-store` garbage collect had failed to recognize the status of my
old user and therefore thought it was still being used, although there had been
no mentions of it in my flake setup!

To counter this issue, manual intervention was required:
1. `sudo rm /nix/var/nix/profiles/per-user/old-user/ -rf`
2. `sudo rm /nix/var/nix/gcroots/per-user/old-user/ -rf`

And afterwards I had ran:
1. `sudo nix-collect-garbage -d`
2. `sudo nix store gc`

To later find out that `21962.08 MiB` had been freed from my system!
1. `1 store paths deleted, 150.24 MiB freed` (`nix-collect-garbage`)
2. `15027 store paths deleted, 21811.84 MiB freed` (`nix store gc`)

# Closing Statement
Be on a look-out for old information that could still be haunting you today. You
never know what the garbage collector fails to clean-up!
