# Project Phase 2

Team Members: Kate Li (kl739), Audrey Holden (aeh252), Katherine Yee (ky424), Julian Correa (jfc297)

Question: What factors predict an NBA player’s salary? Is there a relationship between an NBA player's performance and an increase in their salary? 

In this assignment we will be analyzing how the salaries of NBA players change depending on many different statistical variables, including points, assists, rebounds, blocks, steals, and etc. per game. We will train a multivariable regression to see if we can predict an NBA player's salary depending on their previous season(s) performance statistics. We will also see if there is a positive or negative correlation between performance and salary.

## Data Collection and Cleaning


```python
import requests
from bs4 import BeautifulSoup

import pandas as pd
import numpy as np
import time
```

These are our websites that are stored into variables.


```python
NBA_stats = """https://www.basketball-reference.com/players/"""
ESPN_salaries = """https://www.espn.com/nba/salaries"""
stat_leaders = """https://www.nba.com/stats/leaders?Season=2023-24&SeasonType=Regular+Season"""
kaggle_salary = """https://www.kaggle.com/datasets/thedevastator/exploring-nba-player-performance-and-salaries-19"""
```

We are doing a GET request to the web hosts for the specified filenames. We are ensuring that we are able to make web requests to each website so that we can extract data. 


```python
stats_result = requests.get(NBA_stats)
print(stats_result.status_code)
if stats_result.status_code != 200:
    print("Something went wrong:", stats_result.status_code, stats_result.reason)

salaries_result = requests.get(ESPN_salaries)
print(salaries_result.status_code)
if salaries_result.status_code != 200:
    print("Something went wrong:", salaries_result.status_code, salaries_result.reason)

stat_leaders_result = requests.get(stat_leaders)
print(stat_leaders_result.status_code)
if stat_leaders_result.status_code != 200:
    print("Something went wrong:", stat_leaders_result.status_code, stat_leaders_result.reason)

kaggle_result = requests.get(kaggle_salary)
print(kaggle_result.status_code)
if kaggle_result.status_code != 200:
    print("Something went wrong:", kaggle_result.status_code, kaggle_result.reason)
```

    200
    403
    Something went wrong: 403 Forbidden
    200
    200


We converted each `GET` request with the requests library to a text string using .text and saved the converted outputs into new variables. We used print statements to ensure that we did this step properly.


```python
stats_text = stats_result.text
print(type(stats_text))

salaries_text = salaries_result.text
print(type(salaries_text))

stat_leaders_text = stat_leaders_result.text
print(type(stat_leaders_text))

kaggle_text = kaggle_result.text
print(type(kaggle_text))
```

    <class 'str'>
    <class 'str'>
    <class 'str'>
    <class 'str'>


We converted each HTML text string to a searchable tree of tags using `BeautifulSoup`.


```python
page_one = BeautifulSoup(stats_text, "html.parser")
page_two = BeautifulSoup(salaries_text, "html.parser")
page_three = BeautifulSoup(stat_leaders_text, "html.parser")
page_four = BeautifulSoup(kaggle_text, "html.parser")
```


```python
leaders_info = page_three.find("tr")
print(leaders_info)
```

    None

