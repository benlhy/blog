---
title: "Missing Keys"
date: 2018-01-10T15:12:00+00:00
slug: "missing-keys"
draft: false
cover:
  image: "/images/2018/01/myarch.jpg"
  relative: false
  hiddenInList: false
tags: ["fix", "missing", "keys", "arch", "linux", "upgrade"]
---

Sometimes Arch Linux complains about missing keys, after spending a little too long Googling around, this was the solution:

Upgrade archlinux-keyring first, then upgrade the rest of the system.

```
sudo pacman -Sy archlinux-keyring

```

If that doesn't solve your problems, try switching your keyserver

```
sudo nano /root/.gnupg/dirmngr_ldapservers.conf

```

Change the `keyserver` line to:

```
keyserver hkp://pgp.mit.edu:11371

```

as documented here: [https://wiki.archlinux.org/index.php/Pacman/Package_signing#Cannot_import_keys](https://wiki.archlinux.org/index.php/Pacman/Package_signing#Cannot_import_keys)

Your system time might be wrong as well, so remember to check that.

Sometimes when installing a tool, it says that a particular GPG key is outdated. This means that the tool is no longer maintained and a red flag. You might want to look for keys elsewhere.
