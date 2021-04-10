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

Some of the posts on this blog are pretty big guides or other documents that I like to reference and refer others to regularly. In addition, I have a few other projects that I haven't written about, but would still be nice to share. This post will compile all the bigger posts and projects I've worked on as one easy to reference article.

# stats_can package

I am the creator and maintainer of an open source package called stats_can. It provides a python interface to data and metadata from [Statistics Canada](https://www.statcan.gc.ca/eng/start). You can read the documentation for it [here](https://stats-can.readthedocs.io/en/latest/), the code is available [here](https://github.com/ianepreston/stats_can). You can also see a talk I gave on what goes into building and maintaining the library [here](https://www.youtube.com/watch?v=SJzg7HnISxw). Unfortunately the first few minutes of the talk were not recorded, but the full slide deck is available [here](https://docs.google.com/presentation/d/1ijCHBcqwYRbm3ZuHEJajwq5q89HPCu25UzUV2m-dx7Q/edit?usp=drivesdk).

# Terra Mystica faction selection model

This is a project I worked on for a friend that's really into the board game [Terra Mystica](https://boardgamegeek.com/boardgame/120677/terra-mystica). It scrapes a few hundred thousand games off of an [online Terra Mystica gaming site](https://terra.snellman.net/), ingests and cleans the game data, and then trains a model to help determine which faction you should play, based on the starting layout of the game. Right now the model is a pretty simple linear regression. At some point I'd like to come back to this and try some bayesian methods, both to learn more about them, and to see if I can improve performance. You can see the code for the project [here](https://terra.snellman.net/). I also built a very rudimentary webapp that will let you plug in the starting conditions for the game, and then return a ranked list of all the factions based on those inputs. It's hosted on the free tier of Azure, so it's quite slow, but it's available [here](https://tmmodel.azurewebsites.net/). You can also pull down and deploy the docker container the webapp and model are stored in [here](https://hub.docker.com/repository/docker/ianepreston/terra_mystica).

# Rent or Own model




# Data Structures and Algorithms

My job and interests are both more directly related to data science/engineering than classic software engineering. With that said, I do still like to code, and I think having a solid grasps of at least the basics of CS is handy for anyone who spends a lot of time writing code. To that end, in addition to completing a [Data Structures and Algorithms course at Athabasca University](https://www.athabascau.ca/syllabi/comp/comp272.php) I like to do the [Advent of Code](https://adventofcode.com/) challenges every year:

* Here's my [2020 advent code](https://github.com/ianepreston/advent_2020). This year I was also learning package development and type hinting, so the repository and code are really overengineered for what should be a daily coding kata. It was fun to work on though and I learned a lot
* I didn't fully complete the [2019 advent](https://github.com/ianepreston/advent_2019). That was the year you implemented an assembly language emulator and built on it through the course of the project. Between that (not really my area of interest) and a whole lot of really tedious maze problems near the end (maybe I should come back to those) I didn't really have the motivation to complete the last few days. That was the first year I coded along live though, so that was a good challenge.
* My [2018 advent](https://github.com/ianepreston/advent_2018) is a bit of a weird one. I used the advent challenges as the material for a weekly coding tutorial I ran at work. To facilitate that format some of the code is done in jupyter, and a lot of the solutions are less efficient/elegant than I might have done in another context, but they worked to illustrate whatever concept I was covering at the time.