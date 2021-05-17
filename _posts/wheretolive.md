---
title: "Building a where to live app"
description: "An excuse to teach myself some cool tools and figure out the best place to live"
layout: post
toc: true
comments: true
hide: false
categories: [data, python, yyc]
---

# Introduction

I have two goals with this project:

* Figure out a good place to live when I move next
* Learn some data engineering and system administration type skills

For the first goal, I want to scrape real estate sites in my area and assemble a database of listings. I want to supplement that with open data from the city and other public agencies. I want all of this data to be collected and updated in an automated and efficient process. Finally, I want to be able to analyze this data in order to find the best place to live based on my personal preferences and requirements.

The second goal should come about as a consequence of the first. I have a little practice with web scraping already, but what I want to do that's new this time is to serve the results of that scraping from an api via [fastapi](https://fastapi.tiangolo.com), which I haven't used before. To store the data that I scrape I'll use a database. I've done lots of querying of databases, but I haven't had much opportunity to design one, so this will be a learning experience in the regard. I'll also need to have an [ETL](https://en.wikipedia.org/wiki/Extract,_transform,_load) pipeline to manage the scheduling, ingestion, and other tasks between the scraper and the database. The final analysis part will be mostly techniques I'm already comfortable with, since that sort of thing is my day job currently, but I'm sure I'll pick up some new tricks there as well.

# Scraper

The first piece of any data project of course is to get some data. I'm starting with [RentFaster](https://www.rentfaster.ca/) because that's the main site I've used in the past to look for places. From a little googling I found that they have an api that will return a JSON file of basic data on their listings. I found [an example](https://github.com/furas/python-examples/blob/master/__scraping__/rentfaster.ca%20-%20requests/main.py) of someone scraping it in python and modified it to suit my purposes. Next I turned all the fields in that JSON into a model using [pydantic](https://pydantic-docs.helpmanual.io/). The last step was to make that an api I could call myself by wrapping it in a fastapi app. I'm using [this repository](https://github.com/michael0liver/python-poetry-docker-example) as the template for using poetry to develop my app and the push it to a docker container for deployment.