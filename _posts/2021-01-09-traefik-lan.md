---
title: "Using traefik for internal services"
description: "How to make a reverse proxy work within your LAN"
layout: post
toc: true
comments: true
hide: false
categories: [docker, traefik, networking, tip]
---

## Introduction

I have a small server at home that I use to run various services in [docker](https://www.docker.com/) containers. I don't expose any of them to the internet, I can tunnel in through my [vpn](http://blog.ianpreston.ca/2020/05/06/pfsense.html#openvpn---secure-remote-access) running on my pfsense router. Because of that up until recently it was enough to just publish the ports of my various services and make a bookmark pointing to my server at that port. Recently I wanted to set up a couple services that both expected to be exposed on port 80, the default http port. Just remapping the port would be insufficient, because their clients did not have options to specify a port to connect on. To get around this issue, I decided to suck it up and learn to use a [reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy). I looked into both [nginx](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/) and [traefik](https://traefik.io/) and settled on traefik. Unfortunately for me, pretty much all the tutorials online expect you to be exposing traefik to the web and have all sorts of stuff about TLS and letsencrypt that don't matter to me, and don't really explain how to do the routing if you only want it to work on your internal network. This guide is for me in the future if I ever have to set this up again, and anyone else that's interested in a similar setup.

This guide isn't going to cover what a reverse proxy is, or many of the details of traefik, there are lots of good guides for that online. It's specifically going to cover the configuration specific to a LAN only connection.

## The key configuration

My [pfsense](http://blog.ianpreston.ca/2020/05/06/pfsense.html#dns) box manages DNS and allows me to resolve machines on my network by their name. For instance, my server is named ```mars``` and if I run ```ping mars``` it will correctly resolve to that machine's IP. Traefik doesn't like to play nice with that naming convetion though, at least I couldn't get it to work with all sorts of combinations of ```mars``` and ```mars.localdomain```. Fortunately, I found a [forum post](https://forum.netgate.com/topic/103737/dns-resolver-host-override) that addressed this issue. In my DNS settings I picked a nice sounding domain that I was sure I wouldn't actually want to connect to (in my case I used ian.ca) and set my DNS to redirect any requests to that domain to go to my server. To do this, from pfsense I went to services -> DNS resolver, and added two lines to the "custom options" section near the bottom

```
local-zone: "ian.ca" redirect
local-data: "ian.ca A <my server IP>"
```

## Setting up Traefik

Most of the rest of the traefik setup could be borrowed from other basic guides. I configure traefik and all my other docker containers in an ansible role. You can find that [on my GitHub](https://github.com/ianepreston/recipes/blob/master/ansible/roles/docker/tasks/main.yml). A couple small gotchas that I ran into were making sure Traefik was on the same docker network as the containers I wanted it to route, and to remember the difference between exposing ports (available within the docker network) and publishing ports (mapping them to a port on the host).

## Conclusion

This was one of those setups that is straightforward in retrospect, but I had to spend a lot of time googling and banging my head against the wall before I could get the correct combination of configurations to do what I want. Hopefully this is useful to others, or at least me next time I try and do this.