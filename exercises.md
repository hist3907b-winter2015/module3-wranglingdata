## Exercise 1

We will have looked at TEI in class; now navigate to the [tei-hist3907b](/tei-hist3907) folder and give it a shot! You'll transcribe a page from an abolitionist's pamphlet. You'll also think about ways of transforming the resulting XML into other formats. Make your notes in your open notebook, and upload your XML and your XSL file to your own repository. (As an added optional challenge, create a gh-pages branch and figure out the direct URL to your XML file, and email that URL to me).

## Exercise 2

When we have text that has been marked up, we can do interesting things with it. In the previous exercise, you saw that XSL can be used to add styles to your XML (which a browser can interpret and display). You can see how having those tags makes for easier searching for information as well, right? Sometimes things are not well marked up. Sometimes, it's all a real mess. In exercise two, we'll explore 'regular expressions', (aka 'Regex') or ways of asking the computer to search for *patterns* rather than matching exact text. 

This exercise follows a tutorial written for [The Macroscope](http://themacroscope.org).(Another good reference is [http://www.regular-expressions.info/](http://www.regular-expressions.info/)). Start with [a gentle introduction to regex](/regex.md) and then begin the [regex exercise](/regexex.md). You should have [RegeXr](http://www.regexr.com/), an interactive tutorial that shows us what various regular expressions can do, open in a browser window somewhere so you can test your regular expressions out _first_ before applying them to your data!  We will use the power of regular expressions to search for particular patterns in a published volume of the correspondence of the Republic of Texas. We'll use regex to massage the information into a format that we can then use to generate a social network of letter writers over time.

## Exercise 3

*Open Refine* is the final tool we'll explore in this module. This engine allows us to clean up our messy data. We will feed it the results from Exercise 2 in order to consolidate individuals (ie, 'Shawn' and 'S4awn' are probably the same person, so Open Refine will consolidate that information for us). This exercise also follows a tutorial written for [The Macroscope](http://themacroscope.org).

## Optional exercises

When you're creating files, generating files, downloading files... things get very messy, very quickly. In these optional exercises, we learn some simple commands (in your terminal or shell) to move things around, flatten folder structures, join files together or break them apart.
