---
layout: post
title: Git - Tips and Notes
author: gini
categories: [ cloud ]
tags: [cloud, automation, containers, devops]
permalink: git
featured: false
hidden: false
image: "assets/images/tools-1426927308491-6380b6a9936f.jpeg"
#titleshort: Git
---


## How to clean Git commit history

```shell
# Create Orphan Branch
git checkout --orphan temp_branch

# Add Files to Branch and commit
git add -A
git commit -am "the first commit"

# Delete master Branch 
git branch -D main

# Rename temp/Current Branch to master
git branch -m main

# Push Changes to repo as master
git push -f origin main
```

Original post: [How to Delete Commit History in Github](https://tecadmin.net/delete-commit-history-in-github/)