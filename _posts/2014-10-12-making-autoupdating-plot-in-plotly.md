---
layout: post
title: "Making an auto-updating plot with python and plotly"
excerpt: "Tutorial"
tags: [plotly, beautiful soup, web scraping, python]
share: true
comments: true
---

There are those days when you find tabulated information on the web that is relevant for your homework/assignment/research. Of course, if the owner of this information only cares about its dissemination, you will probably see that there is no figure representing these data visually.

In my case, I needed to visualize meteorological data for the Denver International Airport ([KDEN](http://w1.weather.gov/obhistory/KDEN.html)) uploaded every hour by the National Weather Service. Of cousre, the data are tabulated and there is no figure representing the information. So here is where Plotly comes to play.

One advantage of Plotly is its well documented [API](http://en.wikipedia.org/wiki/Application_programming_interface). If you visit the [Plotly API library](https://plot.ly/api/) you will find APIs for Python, Matlab, R, Node.js, Julia, and ...wait for it...Excel. This means that with any of these programming languages (excluding Excel of course) you can replace your native plotting functions for the Plotly API and upload your plot online.

#First step

I'm very familiar with Matlab and its plotting functions; however, I wanted to try a different approach for what I had in mind. So my first step was start learning Python...or at least the basics. This step should not be difficult to you if you are familiar with [high-level programming languages](http://en.wikipedia.org/wiki/High-level_programming_language) like Matlab and if you have a basic notion about the [Object Oriented Programming](http://en.wikipedia.org/wiki/Object-oriented_programming) (OOP) paradigm. If the last condition does not apply to you, I would recommend reading about OOP before start learning Python.

#Second step

Learning Python was not a "just-for-fun" step, it had a purpose. After some [googling](http://en.wikipedia.org/wiki/Google_%28verb%29) (yes, it seems to be "officially" a verb), I realized that there is something in this world called [web scraping](http://en.wikipedia.org/wiki/Web_scraping). In short, this means that you use your favorite programming language to search and extract data from websites automatically.

So, there was my new vocabulary word when I met [Beautiful Soup](http://www.crummy.com/software/BeautifulSoup/) (BS). As you can guess, BS helps to web-scrap the web using Python. It took me a while to get used to the BS syntax, but I made extensive use of this tutorial made by [Susane mcg](https://www.youtube.com/watch?v=G7RS5BBWxgo) to get the basics that I needed.


#Third step

As [Susane mcg](https://www.youtube.com/watch?v=G7RS5BBWxgo) explain, you can us BS to download a table, save it as a variable, and then search for the specific html tag that contains the data. Therefore, as the Art of War of Sun Tzu says..."know your enemy" (actually I have no idea if it says that). For this, I explored the html structure of the table looking for the table tag that was most useful for extracting the data. This is important because a table might have embedded tables, so you need to look for the one you need.

#Fourth step
Get familiar with Plotly. You need to install the [API for python](https://plot.ly/python/getting-started/)


#Fifth step
Get your hands dirty. We will create a python script called plot_plotly.py. 

First, we import the libraries that we will be using:

{% highlight python %}
import urllib 			# html handling
import datetime			# date handling
import plotly.plotly as py 	# online plotting
from plotly.graph_objs import * # online plotting
from bs4 import BeautifulSoup	# html parsing
import sys 			# command-line argument handling
{% endhighlight %}

A python script can accept arguments from the command-line using sys. Thus, we can use the name of the station (KDEN) as input argument or choose a different station if needed (e.g. KORD):

{% highlight python %}
station=sys.argv[1]
if station!='KDEN':
	print 'WRONG STATION NAME!!'
	exit()
{% endhighlight %}

Then, we use station for parsing and retrieve the url that hosts the table:

{% highlight python %}
urlStr="http://w1.weather.gov/obhistory/"+station+".html"
f=urllib.urlopen(urlStr)
{% endhighlight %}

...and save this to a local variable to be processes by BS:

{% highlight python %}
s=f.read()
{% endhighlight %}

Now it is the turn of BS to work. First, we process the raw html with BS (like when you digest your soup, but the other way around...eww):

{% highlight python %}
soup=BeautifulSoup(s)
{% endhighlight %}

We look for a specific table that has a cell spacing value of 3:

{% highlight python %}
table=soup.find("table",cellspacing="3")
{% endhighlight %}

From this table, we look for all its rows:

{% highlight python %}
dataRows=table.find_all("tr")
{% endhighlight %}

Since the data contained in the table are basically time series of temperature, pressure, etc, we will create vectors containing this information. First, we initialize the variables:

{% highlight python %}
day=[]	# day
hor=[] 	# hour
mnt=[] 	# minute
tmp=[] 	# air temperature
dwp=[] 	# dew point temperature
tmx=[] 	# max temperature 6-h
tmn=[] 	# min temperature 6-h
prs=[] 	# pressure
{% endhighlight %}

Although some people say that this is not necessary in Python, you'll get an initialization error when using these variables in the following loop. This loop populates the vectors with the information extracted from the table:

{% highlight python %}
arrayLen=0
for rowNum, row in enumerate(dataRows):
	rowCells=row.find_all("td")
	if rowCells:
		for cellNum, cell in enumerate(rowCells):
			cellContents=cell.get_text(strip=True) 
			if cellNum==0:
				day.append(int(cellContents))
			if cellNum==1:
				hor.append(int(cellContents[:2]))
				mnt.append(int(cellContents[3:]))
			if cellNum==6:
				try:
					tmp.append(float(cellContents))
				except ValueError:
					tmp.append(float('NaN'))
			if cellNum==7:
				try:
					dwp.append(float(cellContents))
				except ValueError:	
					dwp.append(float('NaN'))
			if cellNum==8:
				try:
					tmx.append(float(cellContents))
				except ValueError:
					tmx.append(float('NaN'))
			if cellNum==9:
				try:
					tmn.append(float(cellContents))
				except ValueError:
					tmn.append(float('NaN'))
			if cellNum==14:
				try:
					prs.append(float(cellContents))
				except ValueError:
					prs.append(float('NaN'))
		arrayLen=arrayLen+1
{% endhighlight %}


We reverse the vectors, so the oldest data resides in the first vector index and the newer in the last:

{% highlight python %}
day.reverse()
hor.reverse()
mnt.reverse()
prs.reverse()
tmp.reverse()
dwp.reverse()
tmx.reverse()
tmn.reverse()
{% endhighlight %}

We can determine the month and year from the system time:

{% highlight python %}
dayNow=datetime.datetime.today().day
monNow=datetime.datetime.today().month
yerNow=datetime.datetime.today().year
mon=[]
yer=[]
for thisDay in day:
	if thisDay>dayNow & monNow==1:
		mon.append(12)
		yer.append(yerNow-1)
	elif thisDay>dayNow:
		mon.append(monNow-1)
		yer.append(yerNow)
	else:
		mon.append(monNow)
		yer.append(yerNow)
{% endhighlight %}

Next, create a time vector:

{% highlight python %}
mydate=[datetime.datetime(year=yer[i],month=mon[i],
			day=day[i],hour=hor[i],minute=mnt[i]) 	
			for i in range(0,arrayLen)]
{% endhighlight %}

Note the embedded loop in the variable above. From what I've learned, that's common practice in Python.

Now that we have the data in vector form, we can make use of Plotly. You need to create an account in Plotly (which is for free) to be able to upload your plot. You will then receive information for logging in from the python script:

{% highlight python %}
py.sign_in("username", "xxxxxxxx")
{% endhighlight %}

There are different options in Plotly to create a time series plot, called trace. Here, I use a single X axis for all the traces and a second Y axis for pressure:

{% highlight python %}
trace1=Scatter(
	name='pressure',
	x=mydate, 
	y=prs,
	mode='lines',
	yaxis='y2'
	)

trace2=Scatter(
	name='temperature',
	x=mydate,
	y=tmp,
	mode='lines'
	)

trace3=Scatter(
	name='dew point',	
	x=mydate,
	y=dwp,
	mode='lines'
	)

trace4=Scatter(
	name='tmax 6-h',	
	x=mydate,
	y=tmx,
	mode='markers'
	)

trace5=Scatter(
	name='tmin 6-h',	
	x=mydate,
	y=tmn,
	mode='markers'
	)

data=Data([trace1,trace2,trace3,trace4,trace5])	
{% endhighlight %}

Then, you can use the following code for the updating time and date of the last plot:

{% highlight python %}
titleStr='<b>National Weather Service - Denver International Airport (KDEN)</b>'
nowStr=datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
updateStr='<br><i>Last Update: '+ nowStr +' (MDT)</i>'
{% endhighlight %}

Next, we create a layout:

{% highlight python %}
layout=Layout(
	title=titleStr+updateStr,
	xaxis=XAxis(title='Time (MDT)'),
	yaxis=YAxis(domain=[0,0.45],
				title='Temperature(F)'),
	yaxis2=YAxis(domain=[0.55,1],
				title='Pressure(hPa)')
	)
{% endhighlight %}

..then make a figure object:

{% highlight python %}
fig=Figure(data=data,layout=layout)
{% endhighlight %}

...and finally send the figure to Plotly:

{% highlight python %}
plot_url=py.plot(fig,
	filename='plot-plotly-example-'+station,
	auto_open=False,
	fileopt='overwrite')
{% endhighlight %}

So, if everything is good, you should obtain a plot like this:


<div class="image-wrap">

<iframe width="560" height="420" frameborder="0" seamless="seamless" scrolling="no" src="https://plot.ly/~rvalenzuela/19/560/420"></iframe>

</div>

#Last step

Although the python script is ready to run, we need a way to run it automaticaly every hour. I learned that there is a Unix utility called [Crontab](http://www.adminschoice.com/crontab-quick-reference) which I found I had in my system by using:

{% highlight bash %}
$ man crontab
{% endhighlight %}

Then, to edit the crontab schedule you type in your terminal:

{% highlight bash %}
$ crontab -e
{% endhighlight %}

This will open a text file using the [Vi](http://en.wikipedia.org/wiki/Vi) text editor. Scheduling the python script to run every 62 minutes for KDEN station would be:

{% highlight text %}
*/62 * * * * /Users/raulvalenzuela/Documents/python/plot_plotly.py KDEN
{% endhighlight %}

#Final thoughts

Of course, there are many ways to do the same job and maybe with an even more efficient approach. However, if you are just learning Python (like me) then this will help you to play around a little bit and get the basic syntax of python.

You might also note that this is not exactly the same plot I showed as example in a [previous post](http://mntmetblog.com/interactive-plots-plotly/). Using this code, every time you run plot_plotly.py new data (if the NWS table is updated) will replace the table you already have in Plotly. Then, you need to work a little more the code in order to keep the existing table and just add the last data from the new NWS table, so your plot will extend until the last time you run the script.










