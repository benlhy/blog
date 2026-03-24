---
title: "Getting Dropbox to work (Linux permissions error)"
date: 2016-11-27T15:25:57+00:00
slug: "getting-dropbox-to-work-linux-permissions-error"
draft: false
tag: ["dropbox", "linux", "permission", "xfce", "thundar", "nautilus", "error", "Guides"]
---

Sometimes you might face a permissions error and reinstalling and uninstalling dropbox doesn't work.

This even happens when you use sudo to run the dropbox daemon. It starts installing, unpacking, and then... nothing. You don't even get a prompt to link your account to Dropbox. Launching from the command line results in an error. Well, if you are using  Thundar or any other file manager, this might be because Dropbox is dependent on Nautilus to run. I am running crouton on a chromebook and this was tried on xenial and jessie distributions.

To get Dropbox to work:

```
sudo apt remove nautilus-dropbox
sudo apt purge nautilus-dropbox
sudo apt install nautilus-dropbox nautilus

```

That's it. The Dropbox login prompt immediately pops up and you can log in. The little dropbox indicator even appears at the top (assuming you use xfce) and you can see the sync progress.

It is a bit wasteful to have two file managers, but Dropbox is useful enough to me that I can bear it. I hope this is of use to you.
