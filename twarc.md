#Twitter Archiving, or TWARC

[Ed Summers](http://inkdroid.org/journal/) is a software developer with feet firmly planted in the digital humanities world. He's currently at the Univeristy of Maryland, in the [Maryland Institute for Technology in the Humanities](http://mith.umd.edu/). Recently, he has built a [python tool](http://mith.umd.edu/) for searching and grabbing tweets (and all their associated metadata, which is extremely rich), called '[Twarc](https://github.com/edsu/twarc)'.

In this optional exercise, you will install Twarc on your machine and use it to download all of the tweets hashtagged with '#hist3907b'. Once you've seen that it's working, you will then consider Twitter as a place for performing history. Perhaps you'll search for tweets related to a current event (the first draft of history, as the journalists used to call their work), perhaps you'll see what people are saying about a historic event, person, or place. Perhaps you'll try to map out how effective/affective those 'historical' twitter accounts are (like the ones that tweet WWII in 'real-time', or that purport to be the Emperor Hadrian).

-----

## Getting set up

The first thing you'll need for this exercise is Python. Python is a programming language heavily used by historians and humanists. You will have noticed that many of the [Programming Historian](http://programminghistorian.org/) tutorials are written in Python. We won't be writing any code; we just need Python on your machine so that your computer understands how to interpret Twarc commands.

1. Download and install [Python](https://www.python.org/downloads/) version 2.7.9. (Twarc will use with Python 2 or 3, but I know it works with 2.7.9 because that's what I have installed on my own machine). Follow all prompts given to you by the installer.
2. Make sure you have [Pip](https://pip.pypa.io/en/latest/installing.html) installed. *NB* Pip *should* be installed when you installed Python - it's included by default in version 2.7.9 or above, so you should be ok (this note is aimed at those of you who already have Python). You can check, on the command line, if pip is installed with this command: ``` pip list ```. This will list every module you've got installed for Python - if Pip is installed, it'll be in the list.
3. Next thing to do is to get the Twarc files from Summers' repository. Go to [https://github.com/edsu/twarc](https://github.com/edsu/twarc) and download as zip. Unzip somewhere easy-to-get-to on your machine. (As it happens, you can also type `` pip install twarc `` from the command line to install it, but it won't give you the utilities folder you'll need later).

## Telling Twitter Who You Are

Twarc works by interacting with Twitter's API. You need to set up authentication with Twitter, so that Twitter knows who you are when you start pinging them, asking for data. I assume you have a Twitter account? If not, you'll need to sign up for Twitter before you can proceed. Make sure you're logged into Twitter. Then,

+ Go to their [developer page and hit the button marked 'create a new app'](https://apps.twitter.com/).

+ You'll be presented with a screen looking rather like this: ![image](https://spring.io/guides/gs/register-twitter-app/images/tw-create-app.png) . In 'name' you have to create a unique name for your Twarc - I've used ``` twarc-SMG ``` for mine. In 'description' call it, 'twarc for my hist3907b class'. In 'website' give it the URL to your open notebook. Finally, you can leave 'call-back url' blank.

+ Agree to terms and conditions, prove you're a human, and click 'create application'.

+ You'll then see a screen that looks like this; here I've already clicked on the 'keys and access tokens' tab: 

![Imgur](http://i.imgur.com/mM4hZNN.png)

+ Copy CONSUMER_KEY, CONSUMER_SECRET, ACCESS_TOKEN, ACCESS_TOKEN_SECRET to a file on your computer; never share this file, don't upload it anywhere, don't put it in your open notebook.

## Using Twarc

Twarc will need that information you wrote down in the step above whenever it calls on Twitter. On a Mac, you can create a new file in the same folder where you unzipped Twarc and store your variables as a shell script. Its contents will look like this:

```
export CONSUMER_KEY="blah"
export CONSUMER_SECRET="blah"
export ACCESS_TOKEN="blah"
export ACCESS_TOKEN_SECRET="blah"
```

....where "blah" is the relevant information (_with_ the quotation marks!), and you save this file as ```enviro.sh```

Then, open a terminal and navigate to your Twarc folder. Type:

```source enviro.sh```

If all goes well, nothing should appear to happen but for a new command prompt appearing.

*Non Mac users* I'm testing solutions for how to do this in other environments. However, Twarc is smart enough that you can also just give it your credentials whenever you run a query, like so:

`twarc.py --consumer_key foo --consumer_secret bar --access_token baz --access_token_secret bez --query ferguson`

_so if you are on a mac, and can set the source as above, you don't need to tell twarc your credentials when you run a search. if you are NOT on a mac, you WILL have to do this._

### A Simple Search

The following would look for tweets concerning Ferguson (that is, tweets with the word or hashtag 'Ferguson', referencing the troubled town in Missouri):

`twarc.py --query ferguson > tweets.json`

(Keep in mind that PC & Linux folks: pass your consumer key and secret etc as per the previous section, when you search).
This command tells your machine to run the code in the Twarc.py file, which asks Twitter to search and return all tweets with 'ferguson' in them, and then to write them to a file called 'tweets.json'. There might be a lot of tweets; this could take some time. (You might try to collect tweets related to Hist3907b instead, as that will be a *much* smaller dataset, just to see what's going on.)

### Extracting Geolocated tweets

There are a number of utilities for Twarc - see [Summer's documentation here](https://github.com/edsu/twarc#utilities). One that you might be interested is the one for grabbing geolocated tweets.

You can output GeoJSON from tweets where geo coordinates are available:

``` utils/geojson.py tweets.json > tweets.geojson ```

This command tells your machine to look in the utils subfolder for a file called geojson.py and to run the code therein on your previously created json file with tweets in it. It filters that file and writes just the ones with location data into the geojson format. 

+ *Instant Map!* Github can recognize geojson and represent it on a map. In a new browser window, and assuming that you are logged into Github, go to [github gist](http://gist.github.com). Drag and drop your tweets.geojson file onto that browser window. Github will upload it. In the 'gist description' write 'geotagged tweets re ferguson'. Save. Instant, clickable, map!

### Your turn

Use Twarc to collect a body of tweets that are historically interesting. Extract the geolocated ones and map them. Use some of the utilities to see if there is a difference between 'male' and 'female' tweeters (and how problematic might that be?). Convert your .json file to .csv and upload it to something like [Voyant](http://voyant-tools.org). Are there trends over time?

(Incidentally, if you get an error message when trying to use one of the utilities, read the error message carefully. Is there something missing that the machine is looking for? [I had such a problem just the other day](https://twitter.com/edsu/status/567505909040304128) Follow the link to see how to solve).
