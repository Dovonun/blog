---
title: Hello World
date: 2023-11-24T16:33:24.126Z
tags:
  - infra
  - kubernetes
  - astro
  - blog
---

In this post, I want to show the journey of this blog.
It all started with the desire to self-host the content, something like [noted](https://noted.lol).

And because I know Kubernetes, I thought it would be a good idea to create a cluster at home to host this project.
It is easy to create a cluster with [k0s](https://k0sproject.io/).
My hardware is just some old gaming rigs I have from friends.
The specs are not spectacular either: a 4th-generation i7 with a couple of gigs DDR3 RAM.

As an OS, I used the current Ubuntu server LTS, and because I have to set up multiple machines, I created a [MAAS server](https://maas.io/) on one of the three machines to install the other two over network boot.
You could argue a MAAS server is overkill to set up two nodes, but then you could have brought up that argument at "Kubernetes for my Blog." You're right in both cases.

Because I lost the proprietary cable to power a SATA SSD with one of the OEM PCs, I had to use 2 power supplies for that one PC for a while. After some time, I powered it with a cable from the other PC.
If you think, "Just change the PSU if you already have an extra one," you would again be correct, but I must disappoint you. The Mainboard does not use a standard 24-pin. Therefore, you would need to change that too.

Anyway, we have a cluster with 3 Nodes, one master, and two worker nodes. I also improved my network situation by using my MAAS as a DNS and DHCP and moving to a 10.0.0.0 network.
To host the blog, I obviously need a location to save my markdown files, and because I have a Kubernetes cluster, I installed [gitea](https://about.gitea.com/) to host my repositories.

I wanted something simple to deploy the blog, so I chose the most used cd tool [argocd](https://argoproj.github.io/cd/).
Because I now have a couple of services, I now need an ingress, or I will eventually run out of IPs. I installed the Nginx ingress controller with Helm because that's what I am familiar with.

Now, other people need to connect to my cluster, and I can't just forward the ports on my router/firewall.
Cloudflare tunnels to the rescue. After a lot of trial and error, I have a setup in which all subdomains on ni-verse.com point to my ingress service through the Cloudflare tunnel.

Finally, I can start to work on the actual blog. The blog should work without JavaScript because it may be available on the darknet someday.
I chose Astro to build this blog to play with some new tech.
[Content Collections](https://docs.astro.build/en/guides/content-collections/) look perfect for this.
Add some tailwindcss and make it black.

Here is the deployment flow: First, I build a docker image.
Then, deploy that image with Argo CD to the cluster.
Here, I had the problem of being unable to push docker images to my gitea container registry.
But right here, 2024 came, and the electricity prices went to the moon.
I stopped my two worker nodes to save money on the power bill.
The one master needs to run, or I can't access the internet anymore.
Because I have to go back to use the DHCP of my router.

But first, I will make this work on GitHub in the meantime.

Continue...
