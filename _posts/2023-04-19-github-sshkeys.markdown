---
layout: post
title:  "SSH Access Keys Setup(new) For Your Github Account"
date:   2023-04-19 00:00:00 +0600
img: post/git-ssh.png
categories: Linux, Git, SSH
comments: true
---

Make github email public: https://github.com/settings/emails

ssh-keygen -t ed25519 -C "your_email@example.com"

eval "$(ssh-agent -s)"

ssh-add ~/.ssh/id_ed25519

cat ~/.ssh/id_ed25519.pub

then add the key to your github account: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account

ssh-keyscan test.rebex.net

add this line to known_hosts file test.rebex.net ecdsa-sha2-nistp256 : https://www.baeldung.com/linux/public-key-known_hosts
