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

```bash
docker-compose up
```

This command will read the docker-compose.yml file and start spinning up the container within. If you've not used nginx before, it'll start pulling down the image before running it. *This might take a few minutes, be patient*.

Eventually you'll see something like `Creating compose_web_1 ... done` and then more output. The container is running!

Open up your web browser and try navigating to http://yourservername:9080 (Note the port number and it's `http`, not `https`. You should hopefully see this screen:

![nginx](/images/nginx.png)

If you see this - congrats! You're using docker-compose to spin up a docker container. 

You'll notice that your terminal is spewing out logs from this as well. If you close your terminal session, it would also stop the container. To stop the container, press `Ctrl+C`. You'll see a messsage like `Stopping compose_web_1 ... done`.

### Docker compose Down

It's a bit rubbish if you need to leave your terminal open all the time, so how do we run our compose file "detatched"? Easy. Run this command:

```bash
docker-compose up -d
```

That `-d` flag means "detached", compose will do exactly what it did before and start the nginx container but in the background, leaving your terminal free for more commands. This time, it should run much faster as well as it's already downloaded the image during the previous steps.

Now, if you go to http://yourservername:9080 again you should get the nginx page from above. Great! But now that it's running in the background, how do we stop it? Easy - 

```bash
docker-compose down
```

You should see some messages about your container being stopped and removed. docker-compose `up` and `down` are the two main commands for spinning the containers up and down.

### Making changes

Let's start up the container again so we can see one of the things that makes compose so powerful. Once again, run this command:

```bash
docker-compose up -d
```

Check that the nginx page is visible at http://yourservername:9080, this is nothing different than what you've done before.

Now, edit the `docker-compose.yml` file in your favourite text editor (I like VS Code, but any text editor will do). We're going to change the port from `9080` to a different number, `9090`. Your file should like like this:

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