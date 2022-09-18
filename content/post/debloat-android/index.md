---
title: Debloat Android
description: A short guide about debloating your damned android phones..
slug: debloat-android
date: 2022-07-28 09:24:00+0000
---

# Introduction
Ever wanted to remove the pre-installed applications that came with your phone?
Fortunately, there is an easy way to debloat your phone by uninstalling the
unnecessary applications installed on your phone rather than disabling them (as
suggested by the application manager of your phone).

# Method
1) Append `programs.adb.enable = true;` to your `configuration.nix` for your NixOS dotfiles.
   a) Or install the package through your distro's package manager.
2) Enable "developer settings" through your settings app and later plug-in your phone to your computer.
3) Launch terminal -> `adb devices` (if device shown) -> `adb shell` + (from your phone) allow access to your phone.
4) `pm list packages` -> list current installed packages.
5) `pm list users` -> to determine where to uninstall the packages from (normal, work or secure folder)
6) `pm uninstall -k --user 0 package-name` -> uninstall package.
   a) If `success` was returned = successfully uninstalled package.
   b) if `unsuccessful` either package does not exist or you mistyped the package name.
