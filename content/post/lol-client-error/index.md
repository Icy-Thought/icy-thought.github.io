---
title: League-Client Not Found
description: Fixing the shitty Riot game..
slug: lol-client-error
date: 2021-04-02 12:31:00+0000
---

#  `LeagueClient.exe` could not be found!
This tutorial is written for Arch Linux users and should be followed with
caution!

## Getting Started
Using Arch Linux as your first distro can be painful sometimes, especially when
you don't know your way around the system. You can get lost, depressed and even
want to through your PC/Laptop from the 8 floor to make sure it doesn't
survive. Although you can reach this certain point, you sure wouldn't commit
suicide after having a Windows 10 update running for 1 day making all your work
delayed!

I wrote this guide to ease the painful process of fixing an error you don't
even know the reason behind it's existence with the help of u/msquirrel who
helped me find the solution to the problem I was facing during the installation
of League of Legends using the script available on Lutris website.

The errors that made the installation crash are the following:

# `Error running C:\Riot Games\League of Legends/Prerequisites/Dx9/dxsetup.exe /silent: child killed: unknown signal`
This error later resulted in the error mentioned in the title of this post. And
after checking the logs while conduction a series of reinstallation of League
of Legends on Lutris we found out there are missing packages that was supposed
to be installed before installing the game using Lutris.

Those packages are:
- `gnutls lib32-gnutls`
- `lib32-alsa-lib`
- `lib32-libldap`
- `lib32-lcms2`
- `samba`
- `krb5 lib32-krb5`
- `lib32-openal`

Copy-Paste in terminal to install the packages:
```
sudo pacman -S gnutls lib32-gnutls lib32-alsa-lib lib32-libldap lib32-lcms2
samba krb5 lib32-krb5 lib32-openal

# After Installing The Required Packages
- Reinstall League of Legends from Lutris.

# Enjoy Playing League!
