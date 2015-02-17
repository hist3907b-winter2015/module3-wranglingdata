#Twitter Archiving, or TWARC

Ed Summers is a software developer with feet firmly planted in the digital humanities world. He's currently at the Univeristy of Maryland, in the [Maryland Institute for Technology in the Humanities](http://mith.umd.edu/). Recently, he has built a [python tool](http://mith.umd.edu/) for searching and grabbing tweets (and all their associated metadata, which is extremely rich), called '[Twarc](https://github.com/edsu/twarc)'.

In this optional exercise, you will install Twarc on your machine and use it to download all of the tweets hashtagged with '#hist3907b'. Once you've seen that it's working, you will then consider Twitter as a place for performing history. Perhaps you'll search for tweets related to a current event (the first draft of history, as the journalists used to call their work), perhaps you'll see what people are saying about a historic event, person, or place. Perhaps you'll try to map out how effective/affective those 'historical' twitter accounts are (like the ones that tweet WWII in 'real-time', or that purport to be the Emperor Hadrian).

-----

## Getting set up.

The first thing you'll need for this exercise is Python. Python is a programming language heavily used by historians and humanists. You will have noticed that many of the [Programming Historian](http://programminghistorian.org/) tutorials are written in Python. We won't be writing any code; we just need Python on your machine so that your computer understands how to interpret Twarc commands.

1. Download and install [Python](https://www.python.org/downloads/) version 2.7.9. (Twarc will use with Python 2 or 3, but I know it works with 2.7.9 because that's what I have installed on my own machine). Follow all prompts given to you by the installer.
2. Make sure you have [Pip](https://pip.pypa.io/en/latest/installing.html) installed. *NB* Pip *should* be installed when you installed Python - it's included by default in version 2.7.9 or above, so you should be ok (this note is aimed at those of you who already have Python). You can check, on the command line, if pip is installed with this command: ``` pip list ```. This will list every module you've got installed for Python - if Pip is installed, it'll be in the list.
3. Next thing to do is to get the Twarc files from Summers' repository. Go to [https://github.com/edsu/twarc](https://github.com/edsu/twarc) and download as zip. Unzip somewhere easy-to-get-to on your machine.

## Telling Twitter Who You Are

Twarc works by interacting with Twitter's API. You need to set up authentication with Twitter, so that Twitter knows who you are when you start pinging them, asking for data. I assume you have a Twitter account? If not, you'll need to sign up for Twitter before you can proceed. Make sure you're logged into Twitter. Then,
1. Go to their [developer page and hit the button marked 'create a new app'](https://apps.twitter.com/).
2. You'll be presented with a screen looking rather like this: ![image](https://spring.io/guides/gs/register-twitter-app/images/tw-create-app.png) . In 'name' you have to create a unique name for your Twarc - I've used ``` SMG-twarc ``` for mine. In 'description' call it, 'twarc for my hist3907b class'. In 'website' give it the URL to your open notebook. Finally, you can leave 'call-back url' blank.
3. Agree to terms and conditions, prove you're a human, and click 'create application'.
4. You'll then see a screen that looks like this; here I've already clicked on the 'keys and access tokens' tab:

