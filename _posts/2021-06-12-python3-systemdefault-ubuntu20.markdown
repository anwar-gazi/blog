---
layout: post
title:  Use Python3 as default python in Ubuntu-20.04
date:   2021-06-12 09:24:13 +0600
description: Use Python3 as default python in Ubuntu-20.04
img: post/ubuntu-python3.jpg
fig-caption: Ubuntu use python3 as default
tags: [Ubuntu, Python3]
categories: ubuntu python3
comments: true
---
TLDR;
```shell
sudo apt install python-is-python3
```

This command effectively does a symlink, but not to any specific python version. Instead it keeps pace with future update.
<br>So you always get the latest python3 as installed by your package manager.
<br>
Original Question at StackOverflow: https://askubuntu.com/questions/1272870/how-can-i-change-the-default-python-on-my-ubuntu-20-04-to-python3-8
