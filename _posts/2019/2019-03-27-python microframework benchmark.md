---
layout: post
title: Benchmark on Python Microframeworks 
date: '2019-11-27 01:00'
excerpt: >-
  We have recently increase the free quota of 20 per day to 600 for our service.
comments: false
cover_image: https://raw.githubusercontent.com/theblackcat102/theblackcat102.github.io/master/images/jonathan-SwVkmowt7qA-unsplash.jpg
---


In the complex world of web development, as the popularity of frontend framework such as React and Vue increase, Python is left to exist to be the language of a backend framework such as Flask and Django.  Django is a fullstack web framework with built in ORM, user management, template renderer and tons of plug and use plugins. While Flask, Sanic are micro framework which are designed to stay lean and fast. So what are the key point developers should consider when choosing their next project’s backend framework?

### Why choose python as the language of backend framework:


Python is Widely used in DL, ML ( Tensorflow, Pytorch, Sklearn, Spacy ), signal processing ( opencv, librosa ), Web ( aiohttp, requests ), Data manipulation and visualization ( pandas, matplotlib ). At currentsapi, all of our data processing is written in Python, so it's trival that we use Python to serve the results as well.

Here's some existing Python backend framework : Django, Flask, Sanic, Bottle, Pyramid etc…

Among all, Django is the most popular framework because:

1. Good middleware layer for simple plug and run

2. Built in ACL and Admin management

3. Built in ORM and database migration 

4. Tons of plug and play plugins, templates, tutorial

5. Django template class makes it simple to show any info.

But Django doesn't always fit your use case because:

1. Complex ORM makes it slow for large query 

    1. Django ORM is slower when compare to other such as peewee

2. High latency and low throughput

    1. Django is last in almost all python backend framework.

    2. While microframework such as Flask and Sanic have great performance

You should only choose a microframework, when lightweight and performance is both your design consideration while not afraid to spend time debugging them.
For example : static website showing news, RESTful API server. Which is why we choose microframework as the start for our API backend, because we need performance and low memory footprint.



## Benchmarking Microframework

We listed 8 most popular micro backend framework from Python and installed them in a linode's Nanode plan. 

1. aiohttp
2. Bottle
3. Flask
4. Japronto
5. Pyramid
6. Sanic
7. Starlette
8. Tornado

## Methodology

All framework are under Linode 1vCPU 1GB with no additional pipeline layer. Adding any pipeline will change the final results.

1. Json serialization and delivering : Since Sanic use ujson by default, ujson is used in all framework for serialization. Hence, the result should correlate to text delivering result as well.

2. Text delivering : Hello World

[Locust](https://locust.io/) is used to load test all frameworks for full 2 minutes. The concurrent users number will incremental increase until the framework starting to drop connection. To prevent os resource cap, resource limitation was set to 999999.

#### Text

| Framework  | Total Request  | Request Per Second  | Median Response Time (ms) | Failed Requests |
|------------|:--------------:|:-------------------:|:---------------------:|:---:|
|   aiohttp   |  18908     |  313.74 | 63 | 0 |
|   Bottle   |  13250    |  221.04 | 120 | 0 |
|   Flask    |  16012 | 265.72  |  120 | 0 |
|   Japronto |  18804 |  313.17 | 62  | 0 |
|   Pyramid  |  12290 |  206.84 | 120 | 345 |
|   Sanic    |  18537 | 310.92  |  120 | 43 |
|   Starlette    |  18671 | 311.90   |  120 | 2 |
|   Tornado  |  18771 |  314.84 | 63 | 0 |
|   Django+Gunicorn  |  18052 |  730 | 152 | 0 |

#### Json

| Framework  | Total Request  | Request Per Second  | Median Response Time (ms) | Failed Requests |
|------------|:--------------:|:-------------------:|:---------------------:|:---:|
|   aiohttp   |  18924     |  317.42 | 63 | 0 |
|   Bottle   |  13402    |  225.04 | 120 | 0 |
|   Flask    |  13393 | 224.14  |  120 | 0 |
|   Japronto |  19003 |  319.82 | 62  | 0 |
|   Pyramid  |  13316 |  223.58 | 120 | 0 |
|   Sanic    |  18525 | 311.76  |  120 | 42 |
|   Starlette    |  17260 | 288.02   |  120 | 3 |
|   Tornado  |  18939 |  317.33 | 63 | 0 |


For the lowest median response time, Japronto and Tornado are the best among all 5 frameworks.

Flask in our opinion are the best synchronous framework among all. aiohttp, Tornado, Japronto have the better performance in async function. Do note that tornado support async function in callback form, while Japronto use uvloop for async task. 

## Other perks

Other than performance, support and maturity are also the factor to consider when chosing to develop your next big idea. Hence, we collected some stats about each microframework for easy comparison. 


| Framework  | WSGI support  |  Github stars | Pypi packages | Support async | Size of maintainer team |
|------------|:--------------:|:--------------:|:---------:|:---------:|:---------:|
|   aiohttp   | yes | 7,140 | 586* | Yes | 2 |
|   Bottle   |  yes | 6,039 | 70* | No | 1 |
|   Flask    | yes  | 42,478 |  642 | No | 4 |
|   Japronto | no  | 6,932 | 2*  | Yes | 1 |
|   Pyramid  | yes | 3,071 | 996* | No | 5 |
|   Sanic    | yes | 11,558 |  155* | Yes | 3 |
|   Starlette    |  yes | 1,250 |  20* | Yes | 1 |
|   Tornado  | yes  | 17,376 | 889* | Yes, callbacks | 2 |

* number based on pypi search results

Stats collected 11/03/2019

# Conclusion

While these number isn't the fastest or most performing backend framework. A light server allows developer to setup a service in one file with little disk usage, is a great option when time and resource are limited.


