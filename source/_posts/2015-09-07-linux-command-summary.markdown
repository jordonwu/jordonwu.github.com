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