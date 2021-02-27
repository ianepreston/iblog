---
title: "Notes on docker-compose"
description: "Tiny snippets of things I forget or often have to look up about docker compose."
layout: post
toc: true
comments: true
hide: false
categories: [docker, tips]
---

## Introduction

This is going to be a grab bag of docker-compose tips and snippets for things that I do commonly enough that I want to write them down but not commonly enough that I remember the syntax offhand.

## Different file names

By default docker-compose wants the compose file to be ```docker-compose.yml``` in the same directory as the command is being run. Generally you want to stick with that, but I did come up with a situation where I wanted to give them different names. You can do ```docker-compose -f <file name> <rest of your commands>``` to get around this. Note that the ```-f <file name>``` has to be at the start, you can't just put it in like any other flag.

## .env files

One of the reasons I was trying to have different file names was because I wanted a bunch of different compose files to share a [.env file](https://docs.docker.com/compose/env-file/). My new solution is to have a ```.env``` file in the root directory of where I keep my compose files and then use symbolic links to make a link to that master file in each directory. Depending on how I created the link though I would run into a ```too many levels of symbolic links``` error. A clean way to solve this is to navigate to the subdirectory and run ```ln -s ../.env .env```. Whatever you put after ```-s``` is literally what's included in the link, so as long as there's a ```.env``` file in the parent folder this will work, regardless of where in your file system you move these directories.

## Conclusion

That's it for now. I'll come back and update this document if anything else comes up.