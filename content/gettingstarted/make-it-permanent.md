---
title: "Making installation permanent"
weight: 10
---

### Modify Go file

To make the installation of compose permanent (to survive reboots), you need to edit your `Go` file. This is a file on your flash drive.

You can access the file via the flash share, which if using SMB will be at `\\YOURSERVER\flash\config\go`

You can also edit it using the terminal by typing `nano /boot/config/go` and pressing enter.

Whatever method you used to install docker compose, you can paste it in here at the bottom of the file. Use # to leave yourself a comment explaining what it is, e.g.

```bash
# Installs Docker compose permanently
COMPOSE_VERSION=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)
curl -L "https://github.com/docker/compose/releases/download/${COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

{{% notice warning %}}
Be careful when editing any system files. While it's relatively easy to undo anything you've done here, it's still an important file.
{{% /notice %}}

Make note of how you edited your Go file as you may want to add some additions to it later.

### Using User Scripts Plugin

First go to the Community Applications tab and find and install the "User Scripts" plugin.

Once installed go to the userscripts page and create a new user scripts. Give it a friendly name such as "installDockerCompose" and edit the script

```bash
#!/bin/bash

## First Install  docker compose
COMPOSE_VERSION=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)
curl -L "https://github.com/docker/compose/releases/download/${COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# make file executable with command beneath
sudo chmod +x /usr/local/bin/docker-compose
```

Click save. You will also want to change the schedule to "At Startup of Array" and now you'll always have docker compose installed.

### Alternative Method Using Docker Container

You can add a docker conatiner of your own by filling out your own template. Provide a name, repository, and other configuration needed. In this case we can use the official [docker/compose](https://hub.docker.com/r/docker/compose) image.

1. Go to Docker tab and select "Add Container".
1. Toggle "Advanced View"
1. Provide a name for your stack i.e. `WebStack`
1. Repository `docker/compose`
1. In Post Arguments you need to provide values to the container. In this example `-f /data/webstack/docker-compose.yml --env-file /data/webstack/.env up -d --build`
1. The docker-compose.yml and .env file are bind-mounted into the container. For this example our docker-compose.yml path is `/mnt/user/appdata/webstack/docker-compose.yml` Select `Add another Path, Port, Variable, Label or Device`.
   - Config Type: Path
   - Name: Data
   - Container Path: /data
   - Host Path: /mnt/user/appdata/
1. Bind-mount the docker socket
   - Config Type: Path
   - Name: docker
   - Container Path: /var/run/docker.sock
   - Host Path: /var/run/docker.sock
