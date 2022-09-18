---
title: Fcitx Sogoupinyin Endless Loop
description: Fixing a not-so-fun fcitx-sogoupinyin endless-loop issue.
slug: sogoupinyin-loop
date: 2021-04-12 00:00:00+0000
---

# Getting Started
Are you a Chinese speaking person? A person planning on learning Chinese
digitally? Did you download Fcitx and set it up to the point that you started
the download for the package fcitx-sogoupinyin but ended up in an endless loop
where you started wondering whether there was an endless reoccurring loop of
errors but didn't believe your gut feelings?

Well, I am here to confirm what you are feeling and of course, this is not the
ultimate solution to the problem because what you might be facing could be
something totally different than the problem I had which was an endless loop of
building Qtwebkit, which is a package that is needed for fcitx-sogoupinyin to
be installed.

The solution to the problem is very simple and goes like this:
1. Cancel the current running installation for fcitx-sogoupinyin by clicking on `CTRL + C`.
2. Install Qtwebkit-bin package from AUR.
3. Reinstall `fcitx-sogoupinyin`.

That should solve the problem for you! Hope you enjoy your time learning/using
the package :)
