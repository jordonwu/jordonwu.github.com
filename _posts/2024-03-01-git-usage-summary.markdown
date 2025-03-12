---
layout: post
title: "Git Usage Summary"
date: 2024-03-01 10:00:00 +0800
comments: true
categories: git github
---

# <center> Git Usage Summary

## How do I use the git cherry-pick command?
In its most basic form, you only need to provide the SHA identifier of the commit you want to integrate into your current HEAD branch:
```
$ git cherry-pick af02e0b
```

This way, the specified revision will directly be committed to your currently checked-out branch. If you would like to make some further modifications, you can also instruct Git to only add the commit's changes to your Working Copy - without directly committing them:
```
$ git cherry-pick af02e0b --no-commit
```

## 99. Reference Link
1> [cherry-pick](https://www.git-tower.com/learn/git/faq/cherry-pick)

