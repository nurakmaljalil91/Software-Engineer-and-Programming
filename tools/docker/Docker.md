---
title: Docker
category: docker
tags:
  - docker
created: 2026-03-28
updated: 2026-03-28
status: active
---

- Login to docker hub

```bash
docker login nurakmaljalil91
```

- Docker pull image

```bash
docker pull nurakmaljalil91/cerxos-web-api:latest
```

- Docker run image

```bash
docker run -it -d --name myapi -p 80:80 nurakmaljalil91/cerxos-web-api:latest
```

- Docker list all containers

```bash
docker ps
```

- Docker stop container

```bash
docker stop myapi
```

- Docker remove all images

```bash
docker system prune -a
```