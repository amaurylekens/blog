+++ 
title = "A football stats scraper" 
date = "2019-10-10" 
author = ""
cover = "" 
tags = ["scraping", "stats"] 
keywords = ["scraping", "stats"] 
description = "This python program allow to recuperate football stats from the web by using selenium" 
showFullContent = false 
+++

{{< fa folder-open >}} [repo](https://github.com/amaurylekens/FootballStatsScraper)

## Motivations

I have often wanted to have a statistics dataset of football matches to be able to apply my statistics courses in a domain that I appreciate and not on the datasets provided by software like R or Python (mpg, iris, .. .).

The first solution I considered was to look for a database or an opensource API containing this type of data. I found several but none satisfied me (size insufficient, not free, ...). So I decided to build myself this database with scraping. 

The advantages of this method are multiple : 

* I get the stats I want
* My dataset increases its size every week when games are added
* It's free

## Goals

The goal of the program is to retrieve tables of this style from the web :

| Goals T1 | Goals T2 | Posse T1 | Posse T2 | Tirs T1 | Tirs T2 | ... |
|:--------:|:--------:|:--------:|:--------:|:-------:|:-------:|:---:|
|     1    |     2    |    45    |    55    |    6    |    8    | ... |
|     3    |     1    |    60    |    40    |    9    |    2    | ... |
|     2    |     2    |    52    |    48    |    3    |    3    | ... |
|    ...   |    ...   |   ...    |    ...   |   ...   |   ...   | ... |


It is on the site [flashscore](https://www.flashscore.com/) that I will realize the scraping. It has several advantages :

* It offers a large number of statistics for a large number of matches
* Each match is identified by a unique identifier and its stats are accessible through this one





