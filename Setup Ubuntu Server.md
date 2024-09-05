## Initial Setup

Log in to remote server using the `ssh` command `ssh root@server-name` and enter password

```bash
ssh root@<server-ip>
```

Once inside fetch update software by running command

```bash
sudo apt-get update
```

Update Ubuntu software by running command

```bash
sudo apt-get upgrade -y
```

- Finally reboot the Ubuntu box by running the `sudo reboot`

```bash
sudo reboot
```

Wait a bit and login back to the server using `ssh` command
## Installing [[Docker]]

Uninstall old version (if have before)

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

Before you install Docker Engine for the first time on a new host machine, set up the [[Docker]] repository. Afterward, install and update [[Docker]] from the repository.

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
```

Add [[Docker]]'s official GPG key:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

Use the following command to set up the repository:

```bash
# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Install [[Docker]] Engine, `containerd`, and [[Docker]] Compose.

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Verify that the Docker Engine installation is successful by running the `hello-world` image:

```bash
sudo docker run hello-world
```

## Install [[PostgreSQL]] packaged by Bitnami

Create directory in `home` called `postgresql`

```bash
mkdir /home/postgresql
```

Create another 3 directories inside `/home/postgresql`

```bash
mkdir /home/postgresql/data
mkdir /home/postgresql/conf
mkdir /home/postgresql/copy
```

For Bitnami container make sure to give permission and ownership for volume data

```bash
sudo chmod 777 copy data conf
sudo chown 1000 copy data conf
```

Go inside `postgresql` directory and create a `yml` file called `docker-compose.yml`

```bash
cd /home/postgresql
nano docker-compose.yml
```

Write this to the `docker-compose.yml`

```yml
version: "3.8"

services:
  postgresql:
    container_name: pg1
    image: "bitnami/postgresql:latest"
    ports:
      - "5432:5432"
    volumes:
      - type: bind
        source: /home/postgresql/data
        target: /bitnami/postgresql
      - type: bind
        source: /home/postgresql/conf
        target: /bitnami/postgresql/conf
      - type: bind
        source: /home/postgresql/copy
        target: /copy
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "5"
    shm_size: 4gb
    environment:
      - POSTGRESQL_PASSWORD=admin4321
```
- Run `docker compose up -d` to run the PostgreSQL container
- You can confirm this by running `docker ps`
![[Pasted image 20230409212708.png]]

## Installing Seq

- Seq is a centralized log file with superpowers.
```yml
version: '3.3'
services:
    seq:
        restart: unless-stopped
        container_name: seq
        environment:
            - ACCEPT_EULA=Y
        volumes:
            - '/home/seq/data:/data'
        ports:
            - '8087:80'
        image: 'datalust/seq:latest'
```
