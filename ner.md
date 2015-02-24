#Using the Stanford NER to tag a corpus

In our regular expressions example, we were able to extract some of the metadata from the document because it was more or less already formatted in such a way that we could write a pattern to find it. Sometimes however clear-cut patterns are not quite as easy to apply. For instance, what if we were interested in the place names that appear in the documents? What if we suspected that the focus of diplomatic activity shifted over time? This is where ‘named entity recognition’ can be useful. Named entity recognition covers a broad range of techniques, based on machine learning and statistical models of language to laboriously trained classifiers using dictionaries. One of the easiest to use out-of-the-box is the Stanford Named Entity Recognizer.  In essence, we tell it ‘here is a block of text – classify!’ It will then process through the text, looking at the structure of your text and matching it against its statistical models of word use to identify person, organization, and locations. One can also expand that classification to extract time, money, percent, and date. 

## Grab the Stanford NER

Let us use the NER to extract person, organization, and locations.  First, download the Stanford NER from http://nlp.stanford.edu/software/CRF-NER.shtml and extract it to your machine. Open the location where you extracted the files. 
+ On a Mac, double-click on the one called ‘ner-gui.command’. (Mac Users: there is also this excellent tutorial from Michelle Moravec [you may wish to consult](http://historyinthecity.blogspot.ca/2014/06/how-to-use-stanfords-ner-and-extract.html).
+ On PC, double-click on ner-gui.bat. This opens up a new window (using Java) with ‘Stanford Named Entity Recognizer’ and also a terminal window. 

Don’t touch the terminal window for now. (*PC users, hang on a moment* – there is a bit more that you need to know before you can use this tool successfully. You will have to use the command line in order to get the output out).

## Running the NER via its GUI

In the ‘Stanford Named Entity Recognizer’ window there is some default text. Click inside this window and delete the text. Then, click on ‘File,’ then ‘Open,’ and select your text for the diplomatic correspondence of the Republic of Texas (that you should still have from earlier modules in this course). Since this text file contains a lot of extraneous information in it – information which we are not currently interested in, including the publishing information and the index table of letters – you should open the file in a text editor first and delete that information. We want just the letters for this exercise. Save with a new name and then open it using ‘File > open’ in the Stanford NER. The file will open within the window. In the Stanford NER window, click on ‘classifier’ then ‘load CRF from file’. Navigate to where you unzipped the Stanford NER folder. Click on the ‘classifier’ folder. There are a number of files here; the ones that you are interested in end with .gz:

*english.all.3class.distsim.crf.ser.gz*<br>
*english.all.4class.distsim.crf.ser.gz*<br>
*eglish.muc.7class.distsim.crf.ser.gz*

These files correspond to these entities to extract:

*3class:	Location, Person, Organization*<br>
*4class:	Location, Person, Organization, Misc*<br>
*7class:	Time, Location, Organization, Person, Money, Percent, Date*<br>

Select the location, person, and organization classifier (ie, 3class) and then press ‘Run NER.’ At this point, the program will appear to ‘hang’ – nothing much will seem to be happening. However, in the background, the program has started to process your text. Depending on the size of your text, this could take anywhere from a few minutes to a few hours. Be patient! Watch the terminal window – once the program has results for you, these will start to scroll by in the terminal window. In the main program window, once the entire text has processed, the text will appear with colour-coded highlighting showing which words are location words, which ones are persons, which ones are organizations. You have now classified a text. Note: sometimes your computer may run out of memory – in that case, you’ll see an error referring to “Out of Heap Space” in your terminal window. That’s OK – just copy and paste a smaller bit of the document, say the first 10,000 lines or so. Then try again.

## Manipulating that data

Mac users can grab the data and paste it elsewhere; PC users will have to run the NER from the command line to get usable output.

### Mac Users

On a Mac, you can copy and paste the output from the terminal window into your text editor of choice. It will look something like: 

LOCATION: Texas<Br>
PERSON: Moore<Br>
ORGANIZATION: Suprema<Br>

And so forth. Once you've pasted it into Textwrangler (or whatever text editor you use), you can now use regular expressions to manipulate the text further. More in a moment.

### PC Users
On a PC, things are not so simple because the command window only shows a small fraction of the complete output – you cannot copy and paste it all! What we have to do instead is type in a command at the command prompt, rather than using the graphical interface, and then redirect the output into a new text file. 

1.	Open a command prompt in the Stanford NER folder on your Windows machine (you can right-click on the folder in your windows explorer, and select ‘open command prompt here’).
2.	Type the following as a single line:

```Java
java -mx500m -cp stanford-ner.jar edu.stanford.nlp.ie.crf.CRFClassifier -loadClassifier classifiers/english.all.3class.distsim.crf.ser.gz -textFile texas-letters.txt -outputFormat inlineXML > “my-ner-output.txt”
```

The first bit, ```java –mx500m``` says how much memory to use. If you have 1gb of memory available, you can type ```java –mx 1g``` (or 2g, or 3g, etc). The next part of the command calls the NER programme itself.  You can set which classifier to use after the ```–loadClassifier classifiers/``` by typing in the exact file name for the classifier you wish to use (you are telling ‘loadClassifier’ the exact path to the classifier). At ```–textFile``` you give it the name of your input file (on our machine, called ```texas-letters.txt```, and then specify the outputFormat.  The ```>``` character sends the output to a new text file, here called ```my-ner-output.txt```.  Hit enter, and a few moments later the programme will tell you something along the lines of

```CRFCLassifier tagged 375281 words in 13745 documents at 10833.43 words per second ```

Open the text file in Notepad++, and you’ll see output like this:

```In the name of the <LOCATION>Republic of Texas</LOCATION>, Free, Sovereign and Independent. To all whom these Presents shall come or may in any wise concern. I <PERSON>Sam Houston</PERSON> President thereof send Greeting```

Congratulations – you’ve successfully tagged a document using a named entity recognizer!
