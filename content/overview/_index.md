---
title: "Overview"
date: 2021-04-06T17:22:21+01:00
chapter: true
---

### Overview

# What _is_ Docker Compose?

It's defined on [the official Docker site](https://docs.docker.com/compose/) as: 

> Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

But what does that mean, really?

Essentially, it lets you define one or more docker containers in a simple text file (using [YAML](https://en.wikipedia.org/wiki/YAML#:~:text=YAML%20(a%20recursive%20acronym%20for,is%20being%20stored%20or%20transmitted.)). These files are easy to read and understand, they can be easily backed up and changes can be made as easily as editing a text file.

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

Built with <i class="fas fa-heart"></i> from Grav and Hugo
