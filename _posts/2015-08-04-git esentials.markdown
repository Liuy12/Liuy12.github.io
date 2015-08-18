---
title: 'git essentials'
comments: true
layout: post
tags:
    - git
    - version control
---
configure user name/email and color.ui

~~~~~~~~~~ git 
git config --global user.name
git config --global user.email 
git config --global color.ui true
~~~~~~~~~~

Basic steps for git,
untracked ----- tracked/stage ------ commit

~~~ git
git init 
git status
git add
git commit -m ""
git push -u origin master
git log
~~~

Pull and merge, pull is basically fetch and merge

~~~ 
git fetch origin pull/ID/head: branchname
git chekcout branchname
git merge 
git mergetool
git push 
~~~

