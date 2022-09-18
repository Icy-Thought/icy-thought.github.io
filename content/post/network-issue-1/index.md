---
title: Network Issue 1
description: Network issues are common and here is one of the solutions that worked for me.
slug: network-issue-1
date: 2021-04-16 23:11:12+0000
---

# Cause?
Weird Power-saving mode enabled by default, which causes Fn-key to not disable
light after being disabled + random network disconnection + bluetooth not
enabling after being disabled although `rfkill list` shows no presence of `soft
kill`.

# Temporary Solution:
```bash
su -c 'ip link set wlp3s0 down && modprobe -r rtw88_8822be && modprobe rtw88_8822be && ip link set wlp3s0 up'
```

# Permantent Solution - Dante777
Append one of the following in the kernel parameters of your bootloader of choice:

## Power-saving Mode
```bash
pcie_aspm.policy=powersave
```

## Performance Mode
```bash
pcie_aspm.policy=performance
```
