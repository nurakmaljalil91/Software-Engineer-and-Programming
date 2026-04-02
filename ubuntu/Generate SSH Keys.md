---
title: Generate SSH Keys
category: ubuntu
tags:
  - ubuntu
created: 2026-03-28
updated: 2026-03-28
status: active
---

you don't have an SSH key pair for GitHub Actions to use:

On your local machine or a secure environment:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com" -f github_actions_key -N   
```

This command generates a 4096-bit RSA key pair named `github_actions_key` (private key) and `github_actions_key.pub` (public key) without a passphrase.

##  Add Public Key to Your Servers


Copy the contents of `github_actions_key.pub` to the `~/.ssh/authorized_keys` file for the user you will SSH as (e.g.,  root) on your **testing server**.