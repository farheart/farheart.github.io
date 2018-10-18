---
layout: post
title: Use BFG to delete large/unwanted files from GIT history
category: GIT
tags: GIT  
keywords: GIT  BFG
published: true
---

# Use BFG to delete large/unwanted files from GIT history

> REF: <https://rtyley.github.io/bfg-repo-cleaner/>

## STEP

### 0 - Download bfg.jar 
<http://repo1.maven.org/maven2/com/madgag/bfg/1.13.0/bfg-1.13.0.jar>


### 1 - clone existing REPO
```bash
$ cd  ~/wy/codes/walmart/dc_realignment/
$ mkdir tmp
$ cd tmp
$ git clone --mirror ../dc_realignment/
# dc_realignment.git/ will be created under tmp/
```


### 2 - run `bfg.jar` to clean large/unwanted files (model.lp + model.mps)

```bash
# Put downloaded bfg.jar under tmp/
$ java -jar  bfg-1.13.0.jar  --delete-files '*.{lp,mps}'  dc_realignment.git

$ gitlog  # to confirm
```


### 3 - clean-up
```bash
$ cd dc_realignment.git
$ git reflog expire --expire=now --all && git gc --prune=now --aggressive 
```


### 4 - push back to mirror source
```bash
# need to git checkout to a different branch on the source
$ git push
```


### 5 - switch back to source and push to remote
```bash
$ cd  ~/wy/codes/walmart/dc_realignment/dc_realignment/
$ git push origin --force
```