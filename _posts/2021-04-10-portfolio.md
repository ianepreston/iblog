---
title: "My Portfolio"
description: "The bigger posts and projects I've worked on that I want to share."
layout: post
toc: true
comments: false
hide: false
search_exclude: false
sticky_rank: 1
---

# Intro

Some of the posts on this blog are pretty big guides or documents that I like to reference and refer others to regularly. In addition, I have a few other projects that I haven't written about, but would still be nice to share. This post will compile all the bigger posts and projects I've worked on as one easy to reference article.

# stats_can package

I am the creator and maintainer of an open source package called stats_can. It provides a python interface to data and metadata from [Statistics Canada](https://www.statcan.gc.ca/eng/start). You can read the documentation for it [here](https://stats-can.readthedocs.io/en/latest/), the code is available [here](https://github.com/ianepreston/stats_can). You can also see a talk I gave on what goes into building and maintaining the library [here](https://www.youtube.com/watch?v=SJzg7HnISxw). Unfortunately the first few minutes of the talk were not recorded, but the full slide deck is available [here](https://docs.google.com/presentation/d/1ijCHBcqwYRbm3ZuHEJajwq5q89HPCu25UzUV2m-dx7Q/edit?usp=drivesdk).

# Terra Mystica faction selection model

This is a project I worked on for a friend that's really into the board game [Terra Mystica](https://boardgamegeek.com/boardgame/120677/terra-mystica). It scrapes a few hundred thousand games off of an [online Terra Mystica gaming site](https://terra.snellman.net/), ingests and cleans the game data, and then trains a model to help determine which faction you should play, based on the starting layout of the game. Right now the model is a pretty simple linear regression. At some point I'd like to come back to this and try some bayesian methods, both to learn more about them, and to see if I can improve performance. You can see the code for the project [here](https://github.com/ianepreston/terra-mystica-models). The notebooks folder in that repository walks through the development process and was used to provide status updates to the friend I was building the app for. I also built a very rudimentary webapp that will let you plug in the starting conditions for the game, and then return a ranked list of all the factions based on those inputs. It's hosted on the free tier of Azure, so it's quite slow, but it's available [here](https://tmmodel.azurewebsites.net/). You can also pull down and deploy the docker container the webapp and model are stored in [here](https://hub.docker.com/repository/docker/ianepreston/terra_mystica).

# Rent or Own model

This package is designed to help me understand the financial trade offs between renting or owning a home. Other online calculators exist, but they're typically designed for the US, and even the Canadian ones I could find were meant for Ontario. Since I live in Alberta, and I like this sort of thing, I thought it would be fun to build my own. You can check out the code [here](https://github.com/ianepreston/rentorown). From the readme on that page, or at [this link](https://mybinder.org/v2/gh/ianepreston/rentorown/HEAD?urlpath=lab/tree/index.ipynb) you can open a [binder](https://mybinder.org/) container that will let you play around with it in a Jupyter notebook. At some point in the future I might turn it into a full fledged web app, but for now the notebook suits my needs.


# Data Structures and Algorithms

My job and interests are both more directly related to data science/engineering than classic software engineering. With that said, I do still like to code, and I think having a solid grasps of at least the basics of CS is handy for anyone who spends a lot of time writing code. To that end, in addition to completing a [Data Structures and Algorithms course at Athabasca University](https://www.athabascau.ca/syllabi/comp/comp272.php) I like to do the [Advent of Code](https://adventofcode.com/) challenges every year:

* Here's my [2020 advent code](https://github.com/ianepreston/advent_2020). This year I was also learning package development and type hinting, so the repository and code are really overengineered for what should be a daily coding kata. It was fun to work on though and I learned a lot
* I didn't fully complete the [2019 advent](https://github.com/ianepreston/advent_2019). That was the year you implemented an assembly language emulator and built on it through the course of the project. Between that (not really my area of interest) and a whole lot of really tedious maze problems near the end (maybe I should come back to those) I didn't really have the motivation to complete the last few days. That was the first year I coded along live though, so that was a good challenge.
* My [2018 advent](https://github.com/ianepreston/advent_2018) is a bit of a weird one. I used the advent challenges as the material for a weekly coding tutorial I ran at work. To facilitate that format some of the code is done in jupyter, and a lot of the solutions are less efficient/elegant than I might have done in another context, but they worked to illustrate whatever concept I was covering at the time.

# Bigger posts

This section will include links to the larger and more involved blog posts I've made.

## Paper Reproduction - "Alberta’s Fiscal Responses To Fluctuations In Non-Renewable-Resource Revenue" in python

This is a paper reproduction I did of a paper that was published by the University of Calgary's school of public policy. In the course of reproducing the paper I actually found a data error in the original paper. After correcting the error the conclusions of the paper do not appear to be supported. An interesting exercise in coding, and reproducibility in science. The blog post is [here](https://blog.ianpreston.ca/econometrics/jupyter/python/alberta/2021/02/26/ferede.html).

## Automating provisioning Arch

I use Arch Linux btw. [This post](https://blog.ianpreston.ca/configuration/linux/arch/2020/11/26/arch-tldr.html) links to a 3 part post series I did about automating the setup of my workstations and servers. It goes from a detailed breakdown of the bash script that's used to get a bare bones arch install, through to using [ansible](https://www.ansible.com/) to do all the system configuration, software install, and server setup (mostly a bunch of docker containers), followed by setting up a user profile using [rcm](https://github.com/thoughtbot/rcm).

## Python packaging guide

There are lots of great guides on how to build a python package. I'm personally a big fan of [hypermodern python](https://cjolowicz.github.io/posts/hypermodern-python-01-setup/) and use its accompanying [cookiecutter](https://cookiecutter.readthedocs.io/en/1.7.2/) whenever I'm setting up a new project. But there's not much out there that walks you through all the phases between just having a script that you can run up to building a full blown package. Also, most guides don't cover conda, and I find the conda docs are really tuned towards people who are bundling C code or something else with their package, not just making a pure python package. To bridge this gap, and to help cement my own understanding of packaging, I wrote [this guide](https://blog.ianpreston.ca/python/poetry/conda/2020/07/09/pypack.html).

## Setting up a data science environment in Windows

[This guide](https://blog.ianpreston.ca/data/python/configuration/2020/02/15/windows-ds-software.html) is for everyone out there that only has a locked down Windows machine, but still wants to work with data in python. Almost all the guides I could find online assumed you had a Mac or Linux machine, and even the Windows ones often assumed you had administrative rights. This guide gets you set up with conda, git, and vs code, all without elevated privilege. The only thing I haven't managed to do in a Windows environment is build a python package. I'm sure it's doable, but this guide is for those writing scripts/notebooks.
