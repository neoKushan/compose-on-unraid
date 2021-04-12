---
title: "Making installation permanent"
date: 2021-04-06T17:28:06+01:00
weight: 10
---

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