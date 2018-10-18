---
layout: post
title: Add Existing Codes to New GIT Repo
category: GIT
tags: GIT
keywords: GIT
published: true
---

# Add Existing Codes to New GIT Repo

> Ref: https://stackoverflow.com/questions/4658606/import-existing-source-code-to-github

## Background

- Local sources codes exist,  want to add / push them to a new remote ((bitbucket/github) GIT repository (without *cloning-then-copying-overwriting*)


## Steps

1. Create the remote repository, and get the URL such as

    `git@github.com:/youruser/somename.git`
    or 
    `https://github.com/youruser/somename.git`

    If your local GIT repo is already set up, skips steps 2 and 3

2. At the root directory of your local source codes
    
    `git init`

    - If you initialize the repo with a .gitignore and a README.md you should do 

        `git pull {url from step 1}`

        to ensure ignore

3. Add and commit the initial repo

    `git add .` 
    `git commit -m 'Add codes'`

4. Attach your remote repo with the name 'origin'
    `git remote add origin {url from step 1}`

5. Execute 
    `git pull origin master`
    to pull the remote branch so that they are in sync.

6. Push up 
    ```bash
    # push all branches
    git push  

    # push master branch
    git push origin master  

    # switch branch and push
    git checkout -b wy
    git push --set-upstream origin wy 
    git push origin wy
    ```