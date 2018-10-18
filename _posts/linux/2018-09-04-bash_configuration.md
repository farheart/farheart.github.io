---
layout: post
title: Ubuntu -- .bashrc configuration
category: Linux
tags: Ubuntu
keywords: Linux  Ubuntu  .bashrc
published: true
---

# Ubuntu -- .bashrc configuration

## `.bash_profile` vs. `.bashrc`
Ref: <https://apple.stackexchange.com/questions/51036/what-is-the-difference-between-bash-profile-and-bashrc>
* `.bash_profile` is executed for **login** shells 
    * When you login (type username and password) via console, either sitting at the machine, or remotely via ssh, `.bash_profile` is executed to configure your shell before the initial command prompt
* `.bashrc` is executed for **interactive non-login** shells
    * If already logged in, just open a new terminal window (xterm) then `.bashrc` is executed before the window command prompt. 
    * `.bashrc` is also run when you start a new bash instance by typing `/bin/bash` in a terminal.
* On OS X, Terminal by default runs a **login** shell every time, so this is a little different to most other systems, but you can configure that in the preferences

## Call `.bashrc` from `.bash_profile`

Add the following to your .bash_profile, you can then move everything into your `.bashrc` file so as to consolidate everything into one place instead of two:
```bash
if [ -f $HOME/.bashrc ]; then
    source $HOME/.bashrc
fi
```

## Template of .bashrc / .bash_profile
```bash
# Proxy setting should be in .bash_profile

# Proxy Setting
export http_proxy="http://xxx.com:8080"
export https_proxy=$http_proxy
export ftp_proxy=$http_proxy  
export rsync_proxy=$http_proxy
export no_proxy="localhost,127.0.0.1,::1,localaddress,.localdomain.com"


# Colorful prompt
export TERM="xterm-color"
export PS1='\n[\[\e[0;96m\]\u\[\e[0m\]@\[\e[0;94m\]\h\[\e[0m\]]:\[\e[0;92m\]\w \n\[\e[0;93m\]\$ \[\e[0m\]'

# Alias
alias ls='ls -al --color=auto'

# GIT
alias gitlog='git --no-pager log --oneline --abbrev-commit --branches=* --graph --decorate --color | head -n 20'

#--------------------------------------------------------
# Below are for reference only
#--------------------------------------------------------

# MacOS only : redefine ls to show details
alias ls='ls -alG'

# Added by Anaconda3 installer
export PATH="/home/wy/soft/anaconda3/bin:$PATH"

# Maven path
export MAVEN_HOME='/Users/y0w02p1/wy/programs/apache-maven-3.5.3'
export PATH=${PATH}:${MAVEN_HOME}/bin

# WY's shell script path
export PATH=${PATH}:~/wy/codes/bash

# Run conda.sh script
. /usr/local/anaconda3/etc/profile.d/conda.sh

```