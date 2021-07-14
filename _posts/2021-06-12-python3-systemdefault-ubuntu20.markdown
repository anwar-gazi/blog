---
layout: post
title:  "Use Python3 as default python in Ubuntu-20"
date:   2021-06-12 09:24:13 +0600
categories: ubuntu python3
---
Bottom line at top: use `sudo apt install python-is-python3`
<br>
This command effectively does a symlink, but its not a rigid symlink. Instead it keeps pace with future update. So you always get the latest python3 as installed by your package manager.
<br>
Original Question at StackOverflow: https://askubuntu.com/questions/1272870/how-can-i-change-the-default-python-on-my-ubuntu-20-04-to-python3-8
