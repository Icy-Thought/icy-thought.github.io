---
title: Baloo File 100% CPU Usage
description: Solving the issue where balooctl consumes 100% CPU power.
slug: baloo-cpu-issue
date: 2021-04-02 00:00:00+0000
---

# Issue
When encountering this issue the computer will start using the CPU to it's
maximum potential which will make the fans noisy and therefore the computer
usage experience terrible.

# Solution
To fix this issue you could either run `balooctl disable` and later remove the
indexing file, created by `baloo_file_index`, or you could edit
`~/.config/baloofilerc` and append the following lines to the end of the
configuration file:

```conf
[General]
only basic indexing=true
```

Most KDE users have a pre-existing `[General]` section and therefore appending
`only basic indexing=true` to the end of that section is enough.

Happy Linuxing!
