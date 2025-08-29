---
layout: post
title: "Git Usage Summary"
date: 2024-03-01 10:00:00 +0800
comments: true
categories: git github
---

# <center> Git Usage Summary

## 1. Command Usage

### branch

1> list local branches
```
$ git branch 
* master
```

2> list remote branches
```
$ git branch -r
  origin-gitlab/master
  origin/HEAD -> origin/master
  origin/master
```

3> list local and remote branches
```
$ git branch -a
* master
  remotes/origin-gitlab/master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```

4> Track remote all branch
```
//Track remote origin all branches
$ remote=origin; for brname in `git branch -r | grep $remote | grep -v master | grep -v HEAD | awk '{gsub(/^[^\/]+\//,"",$1); print $1}'`; do git branch --track $brname $remote/$brname || true; done 2>/dev/null
```

```
//Track remote upstream all branches
$ remote=upstream; for brname in `git branch -r | grep $remote | grep -v master | grep -v HEAD | awk '{gsub(/^[^\/]+\//,"",$1); print $1}'`; do git branch --track $brname $remote/$brname || true; done 2>/dev/null
```

5> Push all branches to remote
```
$ git push -u --all origin
```

6> fetch all branches
```
git fetch --all
```

7> merge branch to main
```
$ git checkout main
$ git merge <branch-to-merge>
```

### tag

1> list local tags
```
$ git tag
v1.0
v2.0
```

2> list remote tags
```
$ git ls-remote --tags origin
53a7dcf1ca57e05d456321b406730b39dc8ed75e        refs/tags/v1.0
7a9ad7fd794bf52a11de43aacc6010978e6100d3        refs/tags/v2.0
```

3> fetch remote tags
```
$ git fetch --all --tags
Fetching origin
From git-repository
   53a7dc..7a9ad7    master     -> origin/master
 * [new tag]         v1.0       -> v1.0
 * [new tag]         v1.0       -> v2.0
```

4> push single tag to remote
```
$ git push <repo-name> <tag-name>
```

5> push all tags to remote
```
$ git push --tags <repo-name>
```

## 2. How do I use the git cherry-pick command?
In its most basic form, you only need to provide the SHA identifier of the commit you want to integrate into your current HEAD branch:
```
$ git cherry-pick af02e0b
```

This way, the specified revision will directly be committed to your currently checked-out branch. If you would like to make some further modifications, you can also instruct Git to only add the commit's changes to your Working Copy - without directly committing them:
```
$ git cherry-pick af02e0b --no-commit
```



## 3. How to remove a big file wrongly committed
```
remote: error: GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com.
remote: error: Trace: 7d51855d4f834a90c5a5a526e93d2668
remote: error: See http://git.io/iEPt8g for more information.
remote: error: File coverage/sensitivity/simulated.bed is 102.00 MB; this exceeds GitHub's file size limit of 100.00 MB
```
Here, you see the path of the file (coverage/sensitivity/simualted.bed).

So, the solution is actually quite simple (when you know it): you can use the filter-branch command as follows:
```
git filter-branch --tree-filter 'rm -rf path/to/your/file' HEAD
git push remote-url branch-name
```

## 99. Reference Link
1> [cherry-pick](https://www.git-tower.com/learn/git/faq/cherry-pick)

2> [How to remove a big file wrongly committed](https://thomas-cokelaer.info/blog/2018/02/git-how-to-remove-a-big-file-wrongly-committed/)
