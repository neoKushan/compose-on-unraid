---
title: "Installing compose"
weight: 1
---

There's a few different ways to install docker compose onto unRAID. There's no wrong method and not really many differences between them as the same compose binaries get installed regardless.

{{% notice note %}}
These methods aren't permanent unless you add them to your Go file. After a reboot, compose will be missing from your machine. However, it'll persist until that next reboot so this is an easy way to test compose out without making permanent changes to your server.
{{% /notice %}}

{{% notice note %}}
You only need to do one of these, whatever one works for you.
{{% /notice %}}

### First Method (using Python)

{{% notice note %}}
This requires python installed. If you've installed the [nerd pack](https://forums.unraid.net/topic/35866-unraid-6-nerdpack-cli-tools-iftop-iotop-screen-kbd-etc/) you probably already have it installed.
{{% /notice %}}

From the terminal, run this command:
```bash
pip3 install docker-compose
```

That's it!

### Second Method (using cURL)

If you don't have python installed, or if you prefer to pull directly from the "source", you can run this snippet from the bash console:

```bash
COMPOSE_VERSION=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)
curl -L "https://github.com/docker/compose/releases/download/${COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose 
chmod +x /usr/local/bin/docker-compose
```

After a few minutes, it'll be installed.

## Testing

The easiest way to test if Compose is installed correctly is to run this command:
```bash
docker-compose --version
```
If it's installed, you'll get a response like `docker-compose version 1.29.0, build unknown`. That means it worked!

## An Alternative way to tinker (Portainer)

If you don't want to mess about installing compose on your server, but you do want to tinker with compose files then you can use [Portainer](https://www.portainer.io/) to deploy a compose yaml file. 

With Portainer installed, simply go to 'Stacks' on the left-hand menu, select '+ Add Stack' and paste your compose file into the web editor.

Installing Portainer is out of scope for this guide, but it's relatively straightforward to install if you're able to install other docker apps.