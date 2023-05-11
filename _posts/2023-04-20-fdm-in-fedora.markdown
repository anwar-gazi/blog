---
layout: post
title:  "Running Free Download Manager in Fedora 37"
date:   2023-04-20 00:00:00 +0600
img: post/fdm-logo.png
categories: Linux
comments: true
---

Free Download Manager(fdm) is good! It has a deb package!!
But I'm on fedora(xfce) :(
So deb2rpm with the package alien. Got an rpm archive, installed but got issues with Qt interface drawing.

Then tried running it from a Ubuntu2210 container with distrobox and podman: https://www.addictivetips.com/ubuntu-linux-tips/how-to-run-ubuntu-programs-on-fedora-linux/
Installed libxcb, exported export QT_QPA_PLATFORM=offscreen : https://github.com/NVlabs/instant-ngp/discussions/300

Still the drawing layer issue.
