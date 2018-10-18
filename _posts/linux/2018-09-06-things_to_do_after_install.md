---
layout: post
title: Things to do after installing new Ubuntu
category: Linux
tags: Ubuntu  
keywords: Ubuntu 
published: true
---

# Things to do after installing new Ubuntu

1. Create `.bash_aliases` or modify `.bashrc`
2. (VBox) Add user to `vboxsf` group

```bash
sudo usermod -aG vboxsf $(whoami)
# Need re-login to be effective
# Can use `groups` to confirm in vboxsf group
```

3. (Optional) Proxy setting
4. Update system

```bash
sudo apt update
sudo apt upgrade
```