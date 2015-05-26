---
layout: post
title: "Adding an existing project to Github"
date: 2015-05-26 08:12:55 +0800
comments: true
categories: git github
---

# 1. Create a new repository on Github #

	Example repo: https://github.com/github-username/test-repo.git

	#To avoid errors, do not initialize the new repository with README, license, or gitignore files. You can add these files after your project has been pushed to GitHub

# 2. Create a local repository on local machine #

	$ mkdir test-repo
	$ cd test-repo
	$ git init
	$ git config --global user.name "Your Name"			# remove --global for current git reop only
	$ git config --global user.email you@example.com	# remove --global for current git reop only
	$ git config --list									# list config
	$ touch test.txt
	$ git add .
	$ git commit -m "First commit"

# 3. Adding a remote repository url to local repository #

	$ git remote -v

	$ git remote add origin https://github.com/github-username/test-repo.git
	or
	$ git remote set-url origin https://github.com/github-username/test-repo.git	# for windows git command

	$ git remote -v
		origin  https://github.com/github-username/test-repo.git (fetch)
		origin  https://github.com/github-username/test-repo.git (push)

# 4. Push the changes in your local repository to Github #

	$ git push origin master
