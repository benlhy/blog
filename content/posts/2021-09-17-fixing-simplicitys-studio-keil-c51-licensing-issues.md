---
title: "Fixing Simplicity's Studio KEIL C51 Licensing Issues"
date: 2021-09-17T17:06:25+00:00
slug: "fixing-simplicitys-studio-keil-c51-licensing-issues"
draft: false
description: "Fixing code size errors and licensing issues in Simplicity Studio"
cover:
  image: "/images/2021/09/sl-logo-blue-code_png_project-main.jpg"
  relative: false
  hiddenInList: false
tags: ["Silicon Labs", "2021", "Simplicity Studio", "KEIL", "C51", "License", "Errors", "fix", "code"]
---

I recently ran into an issue where my code stopped compiling after I added a few new features. After some digging, I realized that KEIL was complaining that the evaluation license that I was using was not valid because the CID code no longer matched, and the code was too big for the evaluation license.

After some digging around I found that if some of your parameters have changed since you applied for the license (like if you updated your computer), then it is possible that the error could occur.
[Licensing User’s Guide: Licensing Errors![](https://www.keil.com/Content/images/Arm_KEIL_horizontal_white_LG.png)](https://www.keil.com/support/man/docs/license/license_errors.htm)
# Removing the old license

Simplicity Studio doesn't allow you to remove the old KEIL license easily from Simplicity Studio. There is no dialog to remove it.

You have to go to: `C:\SiliconLabs\SimplicityStudio\v5\developer\toolchains\keil_8051\9.60\BIN\TOOLS.INI` and delete the existing license code, so the file just contains:

```
[C51]
LIC0=
```

The next time you open Simplicity Studio you will be greeted with the dialog to add a license.

# Obtaining a new license

You can apply for a new licence here. Or use the dialog in Simplicity Studio to apply for a new licence
[Single-User License Management![](https://www.keil.com/Content/images/Arm_KEIL_horizontal_white_LG.png)](https://www.keil.com/license/install.htm)
# Fixing code size

Still, KEIL complained about my code size and ADDRESS SPACE OVERFLOW. It turns out that in the memory model, DATA is fast, but stores only 128 bytes for variables. But if you use XDATA, it is slightly slower but you can store up to 64KB.

It is easy to change the memory model for your project:

- Right click on the Project and select `properties`
- `C/C++ Build > Settings > Tool Settings > General Settings`
- Choose the memory model from the dropdown list

# Compiler Symbol could not be resolved

- Close and reopen the project. This helped me resolve an issue where the 'false' symbol was not being resolved

# Reference
[Migrated to a new SSD this weekend and am now getting license error R201. this error when I build my previously working project: LICENSE ERROR (R201(2): INVALID LICENSE ID CODE (LIC))This was a previously working project. The full message is:![](https://silabs-prod.adobecqms.net/etc.clientlibs/siliconlabs/clientlibs/clientlib-global/resources/images/apple-touch-icon-precomposed.png)|![](https://silabs-prod.adobecqms.net/etc.clientlibs/siliconlabs/clientlibs/clientlib-global/resources/images/redesign-logo.png)](https://silabs-prod.adobecqms.net/community/software/simplicity-studio/forum.topic.html/migrated_to_a_newss-aK4j){{< bookmark
  url="https://community.silabs.com/s/article/address-space-overflow-troubleshooting?language&#x3D;en_US"
  title="Community - Silicon Labs"
  description="The Silicon Labs Community is ideal for development support through Q&A forums, articles, discussions, projects and resources."
  favicon="https://community.silabs.com/favicon.ico?v&#x3D;2"
>}}
