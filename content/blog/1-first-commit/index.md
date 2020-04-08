---
title: "First Commit"
date: "2019-10-15"
description: "How to use git cherry-pick"
---

Recently I made a git commit to the incorrect branch and wondered how to best fix
the situation. After some Googling, I found out about `git cherry-pick`.

How it works:

Get the commit reference on your current branch by using `git log`. Next,
`git checkout` to the branch you had originally meant to make the commit on, and type

```
git cherry-pick commitreference
```

where `commitreference` is a `SHA` that will look
something like `a6f163d340eac0d2c65e362ea18b15e91485a8b2`.

The commit should now be on the correct branch!
