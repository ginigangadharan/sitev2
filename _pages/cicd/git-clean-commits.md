---
layout: post
title: How to clean Git commit history
author: gini
categories: [ cloud ]
tags: [cloud, automation, containers]
permalink: git-clean-commits
featured: false
hidden: false
image: "assets/images/tools-1426927308491-6380b6a9936f.jpeg"
#titleshort: Tools
---


```shell
# Create Orphan Branch
git checkout --orphan temp_branch

# Add Files to Branch and commit
git add -A
git commit -am "the first commit"

# Delete master Branch 
git branch -D master

# Rename temp/Current Branch to master
git branch -m master

# Push Changes to repo as master
git push -f origin master
```

Original post: [How to Delete Commit History in Github](https://tecadmin.net/delete-commit-history-in-github/)