---
title: "Your first Compose file"
date: 2021-04-06T17:28:06+01:00
weight: 5
---

## Overview

In this part of the guide, I'm going to step you through a _very basic_ compose file and the commands necessary to spin up and shut down the container detailed inside it, as well as some hints and tips about what's going on.

## Your first compose file

Here's the most basic compose file I could think of:

```yaml
version: '3.8'

services:
    web:
        image: nginx
        ports:
         - "9080:80"
```

{{% notice info %}}
This compose file expects port `9080` to be available on your server, if a different service is using that port change `9080` to a free port number and for the descriptions below, replace `9080` with the one you've selected.
{{% /notice %}}

This file contains a simple container definition for the nginx web server (the same webserver used in Swag). Don't worry if it doesn't make much sense, we're going to break that down later on.

You'll need save the compose file somewhere on your server as `docker-compose.yml`. I am going to be doing everything from `/mnt/user/appdata/compose` so I have saved the file there, but if you're confident with file paths then you're free to put the file anywhere you like.

{{% notice info %}}
The file name `docker-compose.yml` is a special name that docker-compose will recognise. You can technically name the file anything you want, but you'll have to specify that file every time. For the sake of simplicity, we're going with the default name of `docker-compose.yml`.
{{% /notice %}}

You'll then want to open your terminal / SSH in (Like you did before to install docker-compose) and `cd` to that folder, i.e.

```bash
cd /mnt/user/appdata/compose
```
If you do an `ls`, you should see `docker-compose.yml`.

### Docker Compose up

Now, run the following command:

`docker-compose up`.

This command will read the docker-compose.yml file and start spinning up the container within. If you've not used nginx before, it'll start pulling down the image before running it. *This might take a few minutes, be patient*.

Eventually you'll see something like `Creating compose_web_1 ... done` and then more output. The container is running!

Open up your web browser and try naviging to http://yourservername:9080 (Note the port number and it's `http`, not `https`. You should hopefully see this screen:

![nginx](/images/nginx.png)

If you see this - congrats! You're using docker-compose to spin up a docker container. 

You'll notice that your terminal is spewing out logs from this as well. If you close your terminal session, it would also stop the container. To stop the container, press `Ctrl+C`. You'll see a messsage like Stopping compose_web_1 ... done.

### Docker compose Down

It's a bit rubbish if you need to leave your terminal open all the time, so how do we run our compose file "detatched"? Easy. Run this command:

```bash
docker-compose up -d
```

That `-d` flag means "detached", compose will do exactly what it did before and start the nginx container. This time, it should run much faster as well.

Now, if you go to http://yourservername:9080 again you should get the nginx page from above. Great! But now that it's running in the background, how do we stop it? Easy - 

```bash
docker-compose down
```

You should see some messages about your container being stopped and removed. docker-compose `up` and `down` and the two main commands for spinning the containers up and down.

### Making changes

Let's start up the container again so we can see one of the things that makes compose so powerful. Once again, run this command:

```bash
docker-compose up -d
```

Check that the nginx page is visible at http://yourservername:9080, this is nothing different than what you've done before.

Now, edit the `docker-compose.yml` file in your favourite text edit (I like VS Code, but any text editor will do). We're going to change the port from `9080` to a different number, `9090`. Your file should like like this:

```yaml
version: '3.8'

services:
    web:
        image: nginx
        ports:
         - "9090:80"
```

Note that only that last line is different. Save your changes and go back to your terminal. Once again, run:

```bash
docker-compose up -d
```

Remember, we didn't take our compose container down, we just changed the `yml` and ran `docker-compose up -d` again. You'll notice in the output something like `Recreating compose_web_1 ... done`. You've just changed the port bindings for your container and all it took was editing a text file.

If you had more than one container defined within this text file, compose would be smart enough to _only_ recreate the container that had changed. This is a small sampling of the beauty of using docker-compose.

If you want to tinker with the file, feel free but when you're done, don't forget to run `docker-compose down` again.

{{% notice info %}}
If you've closed your terminal or logged out, remember when accessing the terminal again that you need to `cd` to `/mnt/user/appdata/compose` before you run your docker-compose commands or you'll get errors as it cannot find the docker-compose.yml file.
{{% /notice %}}

## Breakdown of a compose file

Hopefully by now you've undstood how to stand up and spin down a compose file, but what exactly _is_ that yaml file all about? Let's break it down line by line.

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

Within a compose file, you can specify everything that docker is concerned with - from container images, to networks and volumes and some other stuff you might not care about. In this example we're only specifying a container and those go under `services:`. 

This next block is a single container definition:
```yaml
    web:
        image: nginx
        ports:
         - "9090:80"
```

The `web` part is actually just a freeform name - it could be anything, but by default compose will name the container defined below it with `web`. This is actually _super_ useful to know, because you'll find out later on that containers within a single compose file can reference each other using this name. This is also why when you ran `docker-compose up`, it referenced `compose_web_1` - the `compose` part is taken from the folder the docker-compose.yml file is located and the `web` part is this name. We'll talk more about naming containers later, as it's important to know - just know that for now you can write whatever you want in here.

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