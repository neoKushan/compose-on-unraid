---
title: "Drawbacks"
weight: 1
---

If you're reading this guide, it's probably safe to assume that you're familiar enough with how Docker works on unRAID and although "docker is docker is docker", in that a docker container running on one machine should run just as well as a container than runs on another machine, unRAID has some extra niceities on top that don't really work with compose (Or indeed any other method of managing your containers outside the unRAID dashboard). 

None of these are things that should break your machine or showstoppers, but you should at least be aware of them before going in. In later updates to this guide, I'll show you how to mitigate some of these shortcomings.

In no particular order:

## It's more complex (at first)

unRAID's popularity is in part down to how easy it is to use and how easy it is to install additional applications (Docker or otherwise) from a friendly UI. 

Docker-compose is less friendly. It involves editing a text file and using a couple of commands via the terminal. Although this is actually still pretty straightforward, it's not as simple as the way unRAID handles it out of the box. 

Within that extra complexity comes a _lot_ of extra power, but if you're new to all this stuff, there is a learning curve. Hopefully this guide will help you.

## No Icons on your dashboard

Do you like those pretty icons on your unRAID dashboard? Well kiss goodbye to them:

![unRAID Dashboard](/images/unraid-dashboard.png)

Those icons are part of the unRAID-specific templates that the kind community developers create for you. No templates means no icons. If you'd like unRAID to do something about this, feel free to upvote [this feature suggestion](https://forums.unraid.net/topic/105284-use-docker-labels-for-unraid-specific-information-in-docker-templates-to-allow-for-a-11-map-between-unraid-templates-and-docker-compose-files/).

## No "Web UI" links

For the same reason above, the handy "Web UI" link that appears when you click the container's icon won't be there. You'll still be able to stop and start containers, view logs, etc.

![unRAID Dashboard](/images/unraid-no-webui.png)

## Automatic updates of containers is...weird...

I'd almost say that this is broken, but it's not quite. 

Whether it's using the excellent Auto Update Applications plugin, or the unRAID docker dashboard, unRAID *is* able to detect when an update is available for your container and even lets you apply it, but it never marks that container as having actually been updated - so the "update available" prompt persists and the Fix Common Problems plugin will keep alerting you that there are updates available for those containers...forever...

This also means that the "Auto Update Applications" plugin will reset any container that it _thinks_ needs an update on whatever schedule you've set. 

{{% notice warning %}}
To put it bluntly, although plenty of people use docker compose quite successfully on their unRAID servers, **this is not an officially supported solution**. If it all goes _bang_, unRAID/Lime software are under zero obligation to support you.
{{% /notice %}}