---
title: "Overview"
chapter: true
weight: 1
---

### Overview

# What _is_ Docker Compose?

It's defined on [the official Docker site](https://docs.docker.com/compose/) as: 

> Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

But what does that mean, really?

Essentially, it lets you define one or more docker containers in a simple text file (using [YAML](https://en.wikipedia.org/wiki/YAML)). These files are easy to read and understand, they can be easily backed up and changes can be made as easily as editing a text file.

It looks a bit like this:

```yaml
version: "3.9"
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: "redis:alpine"
```
Don't worry if that looks scary, we'll step through what it all means in the rest of this guide.

Be aware, there are some drawbacks to using compose on unRAID, so please read through the following section so you know what you're getting yourself into.

