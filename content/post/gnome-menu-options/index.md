---
title: Gnome Menu Options
description: Controlling our Gnome settings.
slug: gnome-menu-options
date: 2021-10-14 10:23:00+0000
---

# Add/Replace Gnome Menu Options
Want to replace the =Open in Terminal= with =Open in Alacritty= (or any other
terminal)?

## What you need:
1. `FileManager Actions` through:
   ```bash
   sudo pacman -S filemanager-actions
   ```

## Steps
1. Install the `FileManager Actions`.
2. Launch the application and define a new action.
3. Select display item for both `selection` and `location` in context menu.
4. `Action Tab` settings:
   a. `Context label`: the text of the action which will appear in the context menu.
5. `Command Tab` settings:
   a. `Path`: `/usr/bin/alacritty` (replace alacritty with your terminal of choice.)
   b. `Parameters`: `--working-directory=%d/%b`

6. Save the newly created action!

# Congratulations!
