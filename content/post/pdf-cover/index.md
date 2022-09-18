---
title: PDF-Cover
description: Add PDF Cover to Pre-existing Document
slug: pdf-cover
date: 2021-05-06 00:00:00+0000
---

# Procedure
First, convert `image.jpg` to `image.pdf` through:
```bash
convert image.jpg image.pdf
```

Second, combine `image.pdf` with `input.pdf` to produce `output.pdf` through:
```bash
pdfunite image.pdf input.pdf output.pdf
```
