---
layout: post
title: "linux command summary"
date: 2015-09-07 09:18:08 +0800
comments: true
categories: 
---

# 1. How to find files by content #
	$ find "main(" string in *.c file of current path
	$ find . -name "*.c" -print | xargs grep "main("

# 2. Check process memory status #
	$ top 					#using top to get process id
	$ cat /proc/70/status	#70 is the process id
	$ watch -n 10 cat /proc/70/status #check process memory status per 10s
