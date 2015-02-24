# Further Munging the output of NER

So, you've learned how to tag a corpus using the Stanford NER ([here is that exercise again, as a reminder](https://github.com/hist3907b-winter2015/module3-wranglingdata/blob/master/ner.md)). There are several ways we might like to visualize that output.

Unfortunately, depending on what you want to *do* with that data, you are going to have to go back to the data-munging cycle. 
## As a network
Let us imagine that we wanted to visualize the locations mentioned in the letters as a kind of network. You may wish to [examine the exercise introducing Gephi first](https://github.com/hist3907b-winter2015/module4-holes/blob/master/gephi.md).

We will use regular expressions to further manipulate the data into a useful source -> target list.

###REGEX on Mac to Useful Output (PC, please skip to next section):
We need to organize those locations so that locations mentioned in a letter are connected to each other. To begin, we trim away any line that does not start with LOCATION. We do this by using our regular expression skills from module 3, and adding in a few more commands:

FIND: ^(?!.*LOCATION).*$

and replace with nothing. We had a few new commands in there: the ```?!``` tells it to begin looking ahead for the literal phrase LOCATION, and then the dot and dollar sign let us know to end at the end of the line. In any case, this will delete everything that doesn't have the location tag in front. Now, let us also mark those blank lines we noticed as the start of a new letter. 

FIND: ^\s*$

where:
^ marks the beginning of a line<br>
$ marks the end of a line<Br>
\s indicates ‘whitespace’<Br>
\* indicates a zero or more repetition of the thing in front of it (in this case, whitespace)

and we replace with the phrase, “blankspace”. Because there might be a few blank lines, you might see ‘blankspace’ in places. Where you’ve got ‘blankspaceblankspace’, that’s showing us the spaces between the original documents (whereas one ‘blankspace’ was where an ORGANIZATION or PERSON tag was removed).

At this point, we might want to remove spaces between place names and use underscores instead, so that when we finally get this into a comma separated format, places like ‘United States’ will be ‘United_States’ rather than ‘united, states’.

Find:  *<-a single space*<br>
Replace: \_

Now, we want to reintroduce the space after ‘LOCATION:’, so

Find: :_       *<-this is a colon, followed by an underscore*<br>
Replace: :  *<-this is a colon, followed by a space*

Now we want to get all of the locations for a single letter into a single line. So we want to get rid of new lines and LOCATION:

Find: \n(LOCATION:)<br>
Replace: 

It is beginning to look like a list. Let’s replace ‘blankspaceblankspace’ with ‘new-document’.

Find: blankspaceblankspace<br>
Replace: new-document

And now let’s get those single blankspace lines excised:

Find \n(blankspace)<br>
Replace: 

Now you have something that looks like this:

<p>
new-document Houston Republic_of_Texas United_States Texas<br>
new-document Texas<br>
new-document United_States Town_of_Columbia<br>
new-document United_States Texas<br>
new-document United_States United_States_of_ Republic_of_Texas<br>
new-document New_Orleans United_States_Govern<br>
new-document United_States Houston United_States Texas united_states<br>
new-document United_States New_Orleans United_States<br>
new-document Houston United_States Texas United_States<br>
new-document New_Orleans<br>
</p>

Why not leave ‘new-document’ in the file? It’ll make a handy marker. Let’s replace the spaces with commas:

Find:      *<-a single space*<br>
Replace: , 

Save your work as a *.csv file. You now have a file that you can load into a variety of other platforms or tools to perform your analysis. Of course, NER is not perfect, especially when dealing with names like ‘Houston’ that were a personal name long before they were a place name.

###REGEX on PC to Useful Output:
Open your tagged file in Notepad++.  There are a lot of carriage returns, line breaks, and white spaces in this document that will make our regex very complicated and will in fact break it periodically, if we try to work with it as it is. Instead, let us remove all new lines and whitespaces and the outset, and use regex to put structure back. To begin, we want to get the entire document into a single line:

Find: \n<br>
Replace with nothing.

Notepad++ reports the number of lines in the document at the bottom of the window. It should now report lines: 1.

Let’s introduce line breaks to correspond with the original letters (ie, a line break signals a new letter), since we want to visualize the network of locations mentioned where we join places together on the basis of being mentioned in the same letter. If we examine the original document, one candidate for something to search for, to signal a new letter, could be  ```<PERSON>``` to ```<PERSON>``` but the named entity recognizer sometimes confuses persons for places or organizations; the object character recognition also makes things complicated.  Since the letters are organized chronologically, and we can see ‘Digitized by Google’ at the bottom of every page (and since no writer in the 1840s is going to use the word digitized) let’s use that as our page break marker. For the most part, one letter corresponds to one page in the book. It’s not ideal, but this is something that the historian will have to discuss in her methods and conclusions. Perhaps, over the seven hundred odd letters in this collection, it doesn’t actually make much difference.

Find: (digitized)<br>
Replace: \n\1

You now have on the order of 700 odd lines in the document. 

Let’s find the locations and put them on individual lines, so that we can strip out everything else. 

Find: (LOCATION)(.\*?)(LOCATION)<br>
Replace: \n\2

In the search string above, the .\* would look for everything between the first ```<location>``` on the line and the last ```<location>```in the line, so we would get a lot of junk. By using .\*? we just get the text between ```<location> ```and the next ```<location>``` tag.
W
e need to replace the < and > from the location tags now, as those are characters with special meanings in our regular expressions. Turn off the regular expressions in notepad ++ by unticking the ‘regular expression’ box. Now search for <, >, and \ in turn, replacing them with tildes:

```<LOCATION>```Texas```</LOCATION>``` becomes ```~Texas~~~```

Now we want to delete in each line everything that isn’t a location. Our locations start the line and are trailed by three tildes, so we just need to find those three tildes and everything that comes after, and delete. Turn regular expressions back on in the search window.

Find: (~~~)(.*)<br>
Replace with nothing.

Our marker for a new page in this book was the line that began with ‘Digitized by’. We need to delete that line in its entirety, leaving a blank line.

Find: (Digitized)(.\*)<br>
Replace: \n

Now we want to get those locations all in the same line again. We need to find the new line character followed by the tilde, and then delete that new line, replacing it with a comma:

Find: (\n)(~)<br>
Replace: ,

Now we remove extraneous blank lines.

Find: \s*$<br>
Replace with nothing. 

We’re almost there. Let’s remove the comma that starts each line by searching for 
```^,``` (remembering that the carat character indicates the start of a line) and replacing with nothing. Save this as 'cleaned-locations.csv' Congratulations! You’ve taken quite complicated output and cleaned it so that every place mentioned on a single page in the original publication is now on its own line, which means you can import it into a network visualization package, a spreadsheet, or some other tool!

Now you can upload this csv to Gephi [see module 4](https://github.com/hist3907b-winter2015/module4-holes).
