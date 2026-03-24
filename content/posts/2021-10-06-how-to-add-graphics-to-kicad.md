---
title: "How to Add Graphics to KiCAD"
date: 2021-10-06T22:25:43+00:00
slug: "how-to-add-graphics-to-kicad"
draft: false
description: "How to make pretty PCBs in KiCAD using SVGs and Inkscape!"
cover:
  image: "/images/2021/10/mini-2.PNG"
  relative: false
  hiddenInList: false
tag: ["kicad", "2021", "inkscape", "graphics", "svg", "how to", "pretty", "PCB"]
---

# Prerequisites

- Install Inkscape
- Download SVG2Shenzhen - [https://github.com/badgeek/svg2shenzhen](https://github.com/badgeek/svg2shenzhen)

# Install

- Follow the instructions on installing SVG2Shenzhen to Inkscape.

# Use

- SVG2Shenzhen allows you to create vector drawings on multiple layers of KiCAD.
- Start at 1. Prepare Document where you select the layers that SVG to Shenzhen will add to your module.
- Draw on each layer as needed
- Go to 2. Export to KiCAD - select the folder that you want to save the module to, and save as module. Leave the other options at default values. Hit apply.
- Open KiCAD, and add the Module as the footprint library under preferences
- Go to PCBnew and add the footprint of the image to the board.

And that's it!

{{< figure src="/images/2021/10/image-2.png" alt="" caption="Added a graphic onto the board" >}}
