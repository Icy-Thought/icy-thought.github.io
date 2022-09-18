---
title: Enable 'chrome-gnome-shell' in Firefox Nightly
description: A short guide about slapping your PDF's with a cover-page!
slug: chrome-gnome-shell
date: 2021-04-22 00:00:00+0000
---

# Introduction
The process of installing extensions for your gnome-shell is not as simple as
installing addons from your browser store. One has to first install a package
by the name of `GNOME Shell-integration` from the browser store and also
`chrome-gnome-shell` to enable Gnome-extension to install, remove and configure
the installed Gnome extensions.

# Procedure
To enable the installation of Gnome extension on the Firefox browser installed
from nixpkgs we are required to add `chrome-gnome-shell` to our installed
packages in `environment.systemPackages` and later inform the Firefox wrapper
to enable chrome-gnome-shell for our Firefox browser:

```nix
environment.systemPackages = [
  chrome-gnome-shell
];

nixpkgs.config.firefox.enableGnomeExtensions = true;`
```

This solution does not work for Firefox browsers installed through the Mozilla
overlay. Therefore the user ought to link the nix-store generated
`org.gnome.chrome_gnome_shell.json` to `~/.mozilla/native-messaging-hosts/`
everytime `chrome-gnome-shell` it has been updated. Not an ideal solution if
not automated and one of the several methods used to automate such process is
done through appending the following lines to your `home.nix` to allow
Home-Manager to automate the process for you:

```nix
home.file.".mozilla/native-messaging-hosts/org.gnome.chrome_gnome_shell.json".source = "${pkgs.chrome-gnome-shell}/lib/mozilla/native-messaging-hosts/org.gnome.chrome_gnome_shell.json";
    };
```

# Congratulations
You now have a functional solution which enables you to manage your Gnome-shell
extensions! If you have a better solution, please do tell me by either
submitting an issue or a PR informing me about it!
