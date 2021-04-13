---
title: "Benefits"
weight: 2
---

Hopefully you've read about the Drawbacks on the previous page, if not then I **strongly** advise you to read them, however there's a few reasons why it might be worth giving up some of those niceities.

## It's easier

I know some people, perhaps you yourself, look at using the terminal in sheer horror and can't imagine how that is "easy", but when you get the hang of it, it's really really easy to spin up new docker containers or change the configuration of your existing containers - it's literally just editing a text file, a text file that's fairly straightforward to read and running one of maybe 3 commands. 

Conversely, when using any GUI-based docker manager (not just unRAID's), there's lots of fields to fill out and options to set. Compose lets you set those same options, but in an easy and consistent way.

## No reliance on templates

unRAID's community applications are powered by "Templates" and it's one of the things that makes unRAID so great, but it's also a little bit of a drawback. If you've ever wanted to use a certain docker container but there was no community template available, you might find yourself a little bit stuck and unsure of how to proceed. Or perhaps you wanted to use a particular docker image for an application, but there was only a community template for a different fork of that app. 

unRAID makes it super easy to spin up containers using one of those templates, but slightly less so for less popular containers. It's still entirely possible to spin those containers up from unRAID's dockerman, but once you get the hang of compose you'll see how much easier it can be.

To be clear: unRAID's templates are _great_ and I'm a huge fan of anything that reduces the barrier to entry for this technical stuff. If those templates are working for you and you're happy, feel free to stick with them.

However, if you stick with things and learn how to use compose, not only will you no longer be reliant on those templates, you won't even miss 'em!

## Compose is standard and it's everywhere

Not everyone uses unRAID, but quite a lot of people do use docker containers - be that on their PC, on a different NAS, in a VM and so on. Anywhere you can run docker, you can run compose. 

By using compose now, you're no longer locked into unRAID. Unraid as a company could go bankrupt tomorrow and you would be able to take your compose file(s), drop it/them on another machine and run `docker-compose up` to get up and running again.

In fact, aside from the initial setup of installing compose, nothing in this guide is all that specific to unRAID and the same steps would work just as well on a Synology or QNAP NAS.

## It's easy to share configurations

Have you looked on the unRAID forums or reddit and seen posts like "Post your unRAID dashboard" so you can see what apps and containers people are running? Or how about when troubleshooting that awkward Sonarr configuration, with the invariable badly cropped screenshots of their config?

Well that's cool, but wouldn't it be neat if there was an easy way to share the _actual_ configuration? Remember, a compose file is just a text file, you can easily share that! 

My hope is that as more people start using compose, more people will share their compose files with others to help alleviate some of that complex setup - especially for multi-container environments (Like your \*arr's and downloader of choice).

## It's easy to backup your configuration

Want to backup your docker configuration? Just take a copy of that .yml file. That's it. 

(Of course, you still need to back up your application data!)

## Simplified networking

If you've ever had to set up your VPN, your torrent client, your \*arr of choice and tried to make them all talk to each other, then tried to put a reverse proxy in front of it, chances are you've hit a few snags around networking. 

Make no mistake, networking in docker is _complicated_ (Because _networking_ is complicated), but compose is slightly clever - any services you define in a single compose file are treated as one big appplication with its own network. To us, the end user, this doesn't mean much, but it means those containers can "see" each other and communicate with each other with very little faffing on your end. 