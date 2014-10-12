---
layout: post
title: "Making an auto-updating plot with python and plotly"
excerpt: "Tutorial"
tags: [plotly, beautiful soup, web scraping, python]
share: true
---

There are those days when you find tabulated information on the web that is relevant for your homework/assignment/research. Of course, if the owner of this information only cares about its dissemination, you will probably see that there is no figure representing these data visually.

In my case, I needed to visualize meteorological data for the Denver International Airport ([KDEN](http://w1.weather.gov/obhistory/KDEN.html)) uploaded every hour by the National Weather Service. Of cousre, the data are tabulated and there is no figure representing the information. So here is where Plotly comes to play.

One advantage of Plotly is its well documented [API](http://en.wikipedia.org/wiki/Application_programming_interface). If you visit the [Plotly API library](https://plot.ly/api/) you will find APIs for Python, Matlab, R, Node.js, Julia, and ...wait for it...Excel. This means that with any of these programming languages (excluding Excel of course) you can replace your native plotting functions for the Plotly API and upload your plot online.

#First step

I'm very familiar with Matlab and its plotting functions; however, I wanted to try a different approach for what I had in mind. So my first step was start learning Python...or at least the basics. This step should not be difficult to you if you are familiar with [high-level programming languages](http://en.wikipedia.org/wiki/High-level_programming_language) like Matlab and if you have a basic notion about the [Object Oriented Programming](http://en.wikipedia.org/wiki/Object-oriented_programming) (OOP) paradigm. If the last condition does not apply to you, I would recommend reading about OOP before start learning Python.

#Second step

Learning Python was not a "just-for-fun" step, it had a purpose. After some [googling](http://en.wikipedia.org/wiki/Google_%28verb%29) (yes, it seems to be "officially" a verb), I realized that there is something in this world called [web scraping](http://en.wikipedia.org/wiki/Web_scraping). In short, this means that you use your favorite programming language to search and extract data from websites automatically.

eb So there was my new vocabulary word when I met [Beautiful Soup](http://www.crummy.com/software/BeautifulSoup/) (BS). As you can guess, BS helps to web-scrap the web using Python. It took me a while to get used to the BS syntax, but I made extensive use of this tutorial made by [Susane mcg](https://www.youtube.com/watch?v=G7RS5BBWxgo) to get the basics that I needed.


#Third step

As Susane explain, you can us BS to download a table, save it as a variable, and then search for specidic html tag that contains the data. Therefore, as the Art of War of Sun Tzu says..."know your enemy" (actually I have no idea if it says that). For this, I explored the html structure of the table looking for the table tag that was most useful for extracting the data. Notice that a table might have embedded tables, so you need to look for the one you need.

#Fourth step

Get your hands dirty. We will create a python script called plot_plotly.py. In the following, the codes will appear in the same order than in the python script. 

There are a number of libraries we need to import:

{% highlight python %}

import urllib 			# html handling
import datetime			# date handling
import plotly.plotly as py 	# online plotting
from plotly.graph_objs import * # online plotting
from bs4 import BeautifulSoup	# html parsing
import sys 			# command-line argument handling

{% endhighlight %}


The script can accept arguments from the command-line using sys. Thus, we can use the name of the station (KDNE) as input argument or choose a different station if needed (e.g. KORD):

{% highlight python %}
station=sys.argv[1]
if station!='KDEN':
	print 'WRONG STATION NAME!!'
	exit()
{% endhighlight %}

Then, we use the station variable for parsing and retrieve the url that hosts the table:

{% highlight python %}
urlStr="http://w1.weather.gov/obhistory/"+station+".html"
f=urllib.urlopen(urlStr)
{% endhighlight %}

...and save this to a local variable to be processes by BS:

{% highlight python %}
s=f.read()
{% endhighlight %}

Then, it is the turn of BS to work:

{% highlight python %}
# parse html 
soup=BeautifulSoup(s)

# extract table
table=soup.find("table",cellspacing="3")

# from table extract rows
dataRows=table.find_all("tr")
{% endhighlight %}









#Fifth step

Although the python code is ready to run, we need a way to run it automaticaly every hour. Then, I learned that there is a Unix utility called [Crontab](http://www.adminschoice.com/crontab-quick-reference) which I found I had in my system using:


{% highlight bash %}
$ man crontab
{% endhighlight %}

Then, to edit the crontab schedule:

{% highlight bash %}
$ crontab -e
{% endhighlight %}

This will open a text file using [Vi](http://en.wikipedia.org/wiki/Vi). Scheduling the python script to run every 62 minutes for KDEN station would be:

{% highlight text %}
*/62 * * * * /Users/raulvalenzuela/Documents/python/plot_plotly.py KDEN
{% endhighlight %}










