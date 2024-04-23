---
title: Add a background to the Obisidian icon using imagemagick
description: How to add a background to a transparent png image using imagemagick on Linux
slug: imagemagick-icon
date: 2023-10-16 00:00:00+0000
image: 
categories:
    - Personal
tags:
    - GitHub
    - imagemagick
    - commandline
weight: 1
---

Recently I've written a review on a PR where I requested that we used the Obsidian icon from the source. However this icon is only available as an svg file with a transparent background. So I suggested that we used imagemagick to add the background to the image because we already used it in the derivation. This task is however not as trivial as it first might seem. The PR with the discussion can be found [here](https://github.com/nixOS/nixpkgs/pull/260553).

To add the background to the image I came up with the following solution:
```bash
# Create the shadowed image
convert input.png \( +clone -background "rgba(0,0,0,0.5)" -shadow 50x5+5+5 \) +swap -background none -layers merge +repage shadowed_image.png

# Create the rounded mask
convert -size XxY xc:none -fill "#232129" -draw "roundrectangle 0,0,X,Y,35,35" mask.png 

# Overlay the rounded image with the shadow onto the colored background
convert -gravity center mask.png shadowed_image.png -composite -compose SrcIn rounded.png
```

In the solution X and Y denotes the size of the output image.
