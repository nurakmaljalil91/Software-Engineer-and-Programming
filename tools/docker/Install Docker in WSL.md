---
title: Install Docker in WSL
category: docker
tags:
  - docker
created: 2026-03-28
updated: 2026-03-28
status: active
---
## Overview

Instructions for installing Docker Engine on [[WSL]] (specifically for [[Ubuntu Server]]) without using Docker Desktop.

## Installation Steps

### 1. Update Package Index
First, update your existing list of packages:

```bash
sudo apt update
sudo apt upgrade -y
```

### 2. Install Required Dependencies
Install some prerequisite packages which let `apt` use packages over HTTPS:

```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
```

### 3. Add Docker’s GPG Key
Add the GPG key for the official Docker repository to your system:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

### 4. Add Docker Repository
Add the Docker repository to APT sources:

```bash
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 5. Install Docker Engine
Update the package database with the Docker packages from the newly added repo and install:

```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

### 6. Manage Docker as a Non-Root User
To avoid typing `sudo` every time you run the `docker` command, add your username to the `docker` group:

```bash
sudo usermod -aG docker $USER
```
> **Note:** You will need to restart your WSL session (or run `newgrp docker`) for these changes to take effect.

### 7. Start Docker Service
On WSL, you might need to start the Docker service manually if it doesn't start automatically:

```bash
sudo service docker start
```

## Verify Installation

Check the version and run a hello-world container:

```bash
docker --version
docker run hello-world
```

## See Also
- [[Docker]]
- [[WSL]]
