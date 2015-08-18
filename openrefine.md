_This text is adopted from the first drafts of The Macroscope which is currently in-press with Imperial College Press. Users should consult that version once it's published._

(An alternative Open Refine exercise is offered by [Thomas Padilla](http://thomaspadilla.org/dataprep/) and you may wish to give it a try instead/too.) 

# Install Open Refine

>OpenRefine (formerly Google Refine) is a powerful tool for working with messy data: cleaning it; transforming it from one format into another; extending it with web services; and linking it to databases like Freebase.

In this exercise, we are going to use a tool that originated with Google. Since 2012, it has been open-sourced and freely available on the net. Using it takes a bit of getting used to, however. Visit the [Open Refine](http://openrefine.org) home page and watch the three videos. Then, [download it to your machine](http://openrefine.org/download.html)

Follow the installation instructions. Start Open Refine by double clicking on its icon. This will open a new browser window, pointing to http://127.0.0.1:3333. This location is your own computer, so even though it looks like it’s running on the internet, it isn’t. The ‘3333’ is a ‘port’, meaning that Open Refine is running much like a server, serving up a webpage via that port to the browser.

# Start Cleaning our Texan Correspondence.

+ Start a new project by clicking on the ‘create project’ tab on the left hand side of the screen. Click on ‘choose files’ and [select the CSV](http://themacroscope.org/2.0/datafiles/texas-correspondence-OpenRefine.csv) file created in exercise 2. This will give you a preview of your data. Name your project in the box on the top right side (eg., 'm3-exercise3' or similar) and then click ‘create project’. It may take a few minutes.

+ Once your project has started, one of the columns that should be visible in your data is ‘sender’. Click on the arrow to the left of "Sender" in OpenRefine and select Facet->Text Facet. Do the same with the arrow next to "Recipient". A box will appear on the left side of the browser showing all 189 names listed as senders in the spreadsheet. The spreadsheet itself is nearly a thousand rows, so immediately we see that, in this correspondence collection, some names are used multiple times. You may also have noticed that many of the names suffered from errors in the text scan (OCR or Optical Character Recognition errors), rendering some identical names from the book as similar, but not the same, in the spreadsheet. For example the recipient "Juan de Dios Cafiedo" is occasionally listed as "Juan de Dios CaAedo". Any subsequent analysis will need these errors to be cleared up, and OpenRefine will help fix them.

+ Within the "Sender" facet box on the left-hand side, click on the button labeled "Cluster". This feature presents various automatic ways of merging values that appear to be the same.   Play with the values in the drop-down boxes and notice how the number of clusters found change depending on which method is used. Because the methods are slightly different, each will present different matches that may or may not be useful. If you see two values which should be merged, e.g. "Ashbel Smith" and ". Ashbel Smith", check the box to the right in the 'Merge' column and click the 'Merge Selected & Re-Cluster' button below.

+ Go through the various cluster methods one-at-a-time, including changing number values, and merge the values which appear to be the same. "Juan de Dios CaAedo" clearly should be merged with "Juan de Dios Cafiedo", however "Correspondent in Tampico" probably should not be merged with "Correspondent at Vera Cruz." Since we are not experts, we will have to use our best judgement in these cases - or get cracking on some more research to help us make the call. By the end, you should have reduced the number of unique Senders from 189 to around 150. Repeat these steps with Recipients, reducing unique Recipients from 192 to about 160. To finish the automatic cleaning of the data, click the arrow next to "Sender" and select 'Edit Cells->Common transforms->Trim leading and trailing whitespace'. Repeat for "Recipient". The resulting spreadsheet will not be perfect, but it will be much easier to clean by hand than it would have been before taking this step. Click on ‘export’ at the top right of the window to get your data back out as a .csv file.

# Now what?
The text you've just cleaned could now be loaded into something like [Palladio](http://palladio.designhumanities.org/) or [Gephi](http://gephi.org) for network analysis! However, every network analysis program has its own idiosyncracies. Gephi, for instance, can import lists of relationships if the CSV file has columns labelled 'source' and 'target'. So let's assume that's where we want to visualize & analyze this data:

+  In order to get this correspondence data into Gephi, we will have to rename the "Sender" column to "source" and the "Recipient" column to "target". You could do this in a spreadsheet, of course. But since you have Open Refine running.... In the arrow to the left of Sender in the main OpenRefine window, select Edit column->Rename this column, and rename the column "source". Now in the top right of the window, select 'Export->Custom tabular exporter'. Notice that "source", "target", and "Date" are checked in the content tab; uncheck "Date", as it will not be used in Gephi (networks where the nodes have different dates, ie dynamic networks, are fiddly and we will go into them in more detail later). Go to the download tab and change the download option from 'Tab-separated values (TSV)' to 'Comma-separated values (CSV)' and press download.  The file will likely download to your automatic download directory. We will revisit this file later. You can drop it into the Palladio interface right now though! Go ahead and do that. Do you see any interesting patterns? Make a note!

### OPTIONAL: Going further with Open Refine: Named Entity Extraction
Say we wanted, instead of the correspondence network, a visualization of all the places named in this body of letters. It might be interesting to visualize on a map the changing focus of Texas' diplomatic attention over time. There is a plugin for Open Refine that does what is called [Named Entity Extraction](http://en.wikipedia.org/wiki/Named-entity_recognition). The plugin, and how to install & use it, is available [here](http://freeyourmetadata.org/named-entity-extraction/).
+ Use regex on your original document containing the letters to clean up the data so that you have one letter per line.
+ Import the file into Open Refine
+ Extract Named Entities
+ Visualize the results in a spreadsheet
+ Write up a 'how to' in your notebook explaining these steps in detail.

_An interesting use case is discussed [here](http://blog.spaziodati.eu/en/2014/07/24/using-openrefine-to-perform-text-mining-on-your-data-food-for-thoughts/) and [here](http://freeyourmetadata.org/publications/named-entity-recognition.pdf)_

* Further Help * [see this page by Kalani Craig](http://www.kalanicraig.com/2014/12/aha-2015-managing-and-maintaining-digital-data-getting-started-in-digital-history-intermediate-workshop/)

### Optional: Exploring Other Named Entity Extraction tools

*Voyant Tools RezoViz*

[Voyant-Tools](http://voyant-tools) has something called 'RezoViz', which will extract entities and tie them together into a network based on appearing in the same document. Upload your corpus to Voyant-Tools. In the top right, there's a 'save' icon. Select 'url for a different tool/skin'. Select 'RezoViz' from the tools list that pops up. A new URL will appear in the box. Copy, paste into a new browser window. Works best on Chrome.

*Stanford NER*

[Stanford NER](http://nlp.stanford.edu/software/CRF-NER.shtml) download.

+ [Mac instructions for use](http://historyinthecity.blogspot.ca/2014/06/how-to-use-stanfords-ner-and-extract.html). The link is to Michelle Moravec's instructions, for Mac. 

+ Windows: If you're on windows and want to do this, things are a bit more complicated. Download and unzip the NER package. 

+ Open a command prompt in the Stanford NER folder on your Windows machine (you can right-click on the folder in your windows explorer, and select ‘open command prompt here’).

+ Type the following as a single line:

> java -mx500m -cp stanford-ner.jar edu.stanford.nlp.ie.crf.CRFClassifier -loadClassifier classifiers/english.all.3class.distsim.crf.ser.gz -textFile texas-letters.txt -outputFormat inlineXML > “my-ner-output.txt”

The first bit, java –mx500m says how much memory to use. If you have 1gb of memory available, you can type java –mx 1g (or 2g, or 3g, etc). The next part of the command calls the NER programme itself. You can set which classifier to use after the –loadClassifier classifiers/ by typing in the exact file name for the classifier you wish to use (you are telling ‘loadClassifier’ the exact path to the classifier). At –textFile you give it the name of your input file (on our machine, called ‘texas-letters.txt’, and then specify the outputFormat. The > character sends the output to a new text file, here called “my-ner-output.txt”. Hit enter, and a few moments later the programme will tell you something along the lines of

> CRFCLassifier tagged 375281 words in 13745 documents at 10833.43 words per second

Open the text file in Notepad++, and you’ll see output like this:

> In the name of the ```<LOCATION>```Republic of Texas```</LOCATION>```, Free, Sovereign and Independent. To all whom these Presents shall come or may in any wise concern. I ```<PERSON>```Sam Houston```</PERSON>``` President thereof send Greeting

Congratulations! You've tagged a body of letters. What next? You could organize this into XML, you could visualize, you could regex to find and extract all of your locations, or persons, or...
