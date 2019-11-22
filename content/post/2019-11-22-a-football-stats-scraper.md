---
title: A Football Stats Scraper
author: ~
date: '2019-10-10'
slug: a-football-stats-scraper
categories: []
tags: []
description: "This python program allow to recuperate football stats from the web by using selenium" 
---

{{< fa folder-open >}} [repo](https://github.com/amaurylekens/FootballStatsScraper)

# Motivations

I have often wanted to have a statistics dataset of football matches to be able to apply my statistics courses in a domain that I appreciate and not on the datasets provided by software like R or Python (mpg, iris, .. .).

The first solution I considered was to look for a database or an opensource API containing this type of data. I found several but none satisfied me (size insufficient, not free, ...). So I decided to build myself this database with scraping. 

The advantages of this method are multiple : 

* I get the stats I want
* My dataset increases its size every week when games are added
* It's free

# Goals

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

# Technology

To easily scrape into python, I use the selenium package that uses a headless version of chrome to access the web page. To use this package, you must download *chromedriver* available [here](https://chromedriver.chromium.org/downloads).

# Implementation

The software is divided into two main functions :

* A function that retrieves match ids from a flashscore url.
* A function that retrieves the statistics of a match from its id

##### Retrieve match ids

I wrote a python function that from an *flashscore* url that ends with */results/* returns a match id list.

![function_1](/function1.png#center)


The first step was to retrieve the web page linked to the url, which is realized by the following line of code:

```python
    driver.get(url)
    WebDriverWait(driver, 20).until(
    	EC.presence_of_element_located((By.CLASS_NAME, "sportName")))
``` 

This code retrieves the html code of the web page and waits for the javascript to run (it generates essential HTML)

On the site we can see that not all games are displayed directly. You can display more by pressing the *show more matches* button. What is achieved by this code :

```python
    while True:
        more_link = driver.find_elements_by_class_name("event__more--static")
        if more_link == []:
            break
        driver.execute_script("arguments[0].click();", more_link[0])
        time.sleep(wait_time)
``` 

The code executes the javascript associated with the button and waits some seconds

I then identified in the HTML an element that allowed to identify each match individually

```html
<div id="g_1_GE5BMPAg" title="Click for match detail!" 
class="event__match event__match--static event__match--twoLine">...</div>
```

This element is the id attribute of a *&lt;div>* tag that each match has in the HTML.

In python we easily retrieve these ids thanks to the name of the class of these tags.

```python
    match_ids = [match.get_attribute("id")[4:] for
                 match in driver.find_elements_by_class_name("event__match--static")]
``` 

By nesting these elements we can achieve the purpose of the function. The complete code is available in the repo.


##### Retrieve stats with a id

I wrote a second python function that allows to get the stats of a match from his id.

![function_2](/function2.png#center)

The first step is to load the page and wait for the javascript to run.

```python
    match_path = "{}/#match-statistics;0".format(match_id)
    match_path = "https://www.flashscore.com/match/{}".format(match_path)
    driver.get(match_path)
    WebDriverWait(driver, 60).until(EC.presence_of_element_located(
        (By.ID, "tab-statistics-0-statistic")))
``` 

Then you have to get the names of stats and the values of these stats for both teams.

```python
        stat_titles = [stat.text for stat in
                       driver.find_elements_by_class_name("statText--titleValue")]
        home_stats = [home_stat.text for home_stat in
                      driver.find_elements_by_class_name("statText--homeValue")]
        away_stats = [away_stat.text for away_stat in
                      driver.find_elements_by_class_name("statText--awayValue")]
``` 

Then we select the statistics we need in the set of statistics retrieved.

These two functions used one after the other allow to build the tables from urls and the names of the desired stats.



