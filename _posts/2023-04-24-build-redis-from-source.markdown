---
layout: post
title:  "Build Redis From Source | Fedora-37"
date:   2023-04-24 00:00:00 +0600
img: post/redis.png
categories: fedora, redis
comments: true
---

Software center or binary downloads save some time in getting a workable package up and running.
<br>
But the saved time is wasted mostly in non productive works like "leisure" and entertainment.
<br>
Building from source is not particularly complex, we are just not used to doing this.
<br>
Building redis is very straightforward. No need to copy paste the instructions here.
<br>
The source readme file is sufficient.
<br>
Afrer compiling, I usually do not put the binary in the root filesystem.
<br>
Instead keeping that binary in another partition, and then use alias.
<br>
```bash
touch ~/.bash_aliases
echo "alias redis='/path/to/bin/redis-server'" > ~/.bash_aliases
vim .bashrc
# copy these lines and put in .bashrc
# start copy
if [ -e $HOME/.bash_aliases ]; then
    source $HOME/.bash_aliases
fi
# end copy

# now reload the .bashrc
source ~/.bashrc
```
Now you got access to the redis binary with just `redis` command

thanks: https://opensource.com/article/19/7/bash-aliases

