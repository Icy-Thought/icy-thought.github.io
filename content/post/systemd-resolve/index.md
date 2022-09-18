---
title: Setting up systemd-resolved
description: A short guide about setting up systemd-resolve on your system!
slug: systemd-resolved
date: 2021-05-06 12:34:00+0000
---

# Procedure
1. Edit `/etc/systemd/resolved.conf` by adding the valuable bits (dns, fallback dns)
2. Execute the follwing commands in your terminal instance:
   a. `systemctl restart systemd-resolved`
   b. `sudo ln -s /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf`
