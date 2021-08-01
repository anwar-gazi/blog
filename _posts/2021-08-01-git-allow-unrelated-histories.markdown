---
layout: post
title:  "Git Merge Your Repo With Your GitHub Repo(both started seperately)"
date:   2021-08-01 13:00:00 +0600
img: post/django-custom-user-model.png
categories: git tutorials
comments: true
---

You started coding your projects locally, and created its git repo(not empty) seperately initiated with README and LICENSE files. 

<br>

So before you merge, this are basically two projects and hence git refuse to mergeby default with a git pull.

<br>

If your GitHub repo was created as blank i.e., without any files that it wouldn't be a problem. But now your local repo has your initial codes and GitHub repo has README and LICENSE files.

<br>

And git pull shows the error
```shell
fatal: refusing to merge unrelated histories
```

Of course you can solve this with either
 - Go back, create empty repo at GitHub
 - Go back, create GitHub repo with the initial files, then clone/pull it and then add your initial codes in the local repo

But if you think like me(no going back, do with what I'm with now), the solution is simpler. Use the flag --allow-unrelated-histories for git-pull.
<br>
So with your initial local codes committed, get your remote repo files(README and LICENSE) with
```shell
git pull --allow-unrelated-histories origin main
```

