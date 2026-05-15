---
title: Linux
category: linux
tags:
  - linux
created: 2026-03-28
updated: 2026-05-14
status: active
---

## Distros

- [[Pop!_os]] — previous OS on HP ZBook Power G9
- [[CachyOS]] — current OS on HP ZBook Power G9 (Arch-based, Hyprland + GRUB)
## List all installed packages on an Ubuntu

To list all installed packages on an Ubuntu system, you can use the following command:

```bash
dpkg --list
```

Or, if you prefer a more detailed output:

```bash
dpkg -l
```

Alternatively, if you are using `apt` and want a simpler list:

```bash
apt list --installed
```

Each of these commands will display a list of installed packages, including version information.

## Find number of file in Linux

```bash
find . -type f | wc -l
```

## Check IP Address
  
```bash
$ hostname -I

$ ip a

$ ip addr show
```

- use `hostname -I` will give the IP address of the system only
## Shutdown now

```bash
sudo shutdown now
```