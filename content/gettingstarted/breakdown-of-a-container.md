---
title: "Breakdown of the example compose file"
date: 2021-04-06T17:28:06+01:00
weight: 6
---

## Breakdown of a compose file

Hopefully by now you've undstood how to stand up and spin down a compose file, but what exactly _is_ that yml file all about?

Well, firstly, what _is_ YAML? Essentially, it's a very simple language that is easy for both humans and computers to understand. It's pretty forgiving in terms of how it's formatted and how you specify some values, for example you might see some values enclosed in double quotes like "8080:80", but you can equally use single quotes such as `8080:80`. You can indent the file using tabs or spaces and as long as it's consistent, it will usually work. 

For a bigger overview of what YMAL is, there's a good walkthrough [here](https://www.cloudbees.com/blog/yaml-tutorial-everything-you-need-get-started/) but don't worry too much about it, as you'll see it's fairly easy to understand just by reading some examples.

So on that note, let's break the example file down line by line.

```yaml
version: '3.8'

services:
    web:
        image: nginx
        ports:
         - "9090:80"
```

The first line is the version of the compose specification (Not docker-compose itself):

```yaml
version: '3.8'
```

What's special about 3.8? Well, nothing really. It's just the latest version of the compose specification at the time I wrote this guide. One of the good things about compose is that they can add (and remove!) parts of the specification without breaking too much becuase the file itself is versioned. You don't need to have a specific version of compose installed, just the latest works fine and you can upgrade any time. However, this means at the top of your file you need to specify the version. If you're unsure, just use 3.8 but be aware if you're looking at tutorials and examples from around the net that some things shift between versions and that there are some fairly major breaking changes between 2.0 and 3.0 especially.

You can read more about the different compose file versions [here](https://docs.docker.com/compose/compose-file/compose-versioning/).

```yaml
services:
```

Within a compose file, you can specify everything that docker is concerned with - from container images, to networks and volumes and some other stuff you might not care about. In this example we're only specifying a container and those go under `services:`. Larger, more complex compose files will have things like `networks:` and `volumes:` but for simplicity, the thing we're looking at only has a single container defined here, under this `services:` block.

This next block is a single container definition:
```yaml
    web:
        image: nginx
        ports:
         - "9090:80"
```

The `web` part is actually just a freeform name - it could be anything, but by default compose will name the container defined below it with `web`. This is actually _super_ useful to know, because you'll find out later on that containers within a single compose file can reference each other using this name. This is also why when you ran `docker-compose up`, it referenced `compose_web_1` - the `compose` part is taken from the folder the docker-compose.yml file is located and the `web` part is this name. We'll talk more about naming containers later, as it's important to know - just know that for now you can write whatever you want in place of `web`.

The `image: nginx` part is the docker image itself that it pulls from the docker hub. It could have been `image: linuxserver/swag` instead and you can include tags from images, such as `image: nginx:latest` or versions, like `image: nginx:1.19.9`.

Finally, we have the `ports:` section. This is where you specify any and all ports you're binding from your host to your container. The nginx web server always listens on port `80`, but we bound port `9090` in this example. This is a longwinded way of saying that port `9090` of the host (your unraid server) binds to port `80` on the container. That's why browsing http://yourservername:9090 brought up the nginx page.

We can specify multiple ports with additions to this list, for example:

```yaml
    web:
        image: nginx
        ports:
         - "9090:80"
		 - "8443:443"
		 - "1234:1234"
```

And that's it! Remember, this is a very basic compose file, there's lots more to go into it later (Such as volumes or environment variables) but hopefully that wasn't too scary. 

We'll do a much more in-depth compose breakdown later as we tinker with bigger and more complicated examples.