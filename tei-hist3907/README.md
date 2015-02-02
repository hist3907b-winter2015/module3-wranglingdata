This worksheet, and all related files, are released CC-BY. 
By M. H. Beals; Adapted for HIST3907b by S Graham

# Close Reading with TEI #

This exercise will explore a historical text and help you create a digital record of your analysis

### Vetting a Website ###

Visit the Recovered Histories Website at http://www.recoveredhistories.org

Examine the site's layout and read its introduction  
+ What makes you believe this site is a trustworthy provider of historical texts?
+ What makes you believe this site is NOT a trustworthy provider of historical texts? 
 
### Finding a Source ###

Visit the site's collections via the 'Browse' function

Locate the pamphlet *Negro Slavery* by Zachary Macaulay and open it

This is an abolitionist pamphlet regarding the Atlantic slave trade, presenting and examine evidence of how it is run.  When you approach a primary source like this, it is tempting to read through it from beginning to end, to get an overview of its contents, and then 'mine' or 'cherry-pick' good quotations to include in your assessments.  However, we are going to focus on examine a very small part of the text in a very high level of detail.  

### Setting Up Your Workspace ###

Arrange your workspace so that you have the scanned text of the pamphlet easily visible on one side of your screen. Open the 'blanktemplate.txt' file in Textwrangler or Notepadd++ (or any text editor that understands encoding) and have that on the other side of the screen.

The last lines will be 
<code>&lt;/body&gt;&lt;/text&gt;&lt;/TEI&gt;&lt;/teiCorpus&gt;</code>. Everything you write today should be just above <code>&lt;/body&gt;</code>.

### Transcribing Your Page ###

The first thing you will need is go to the tag 

<code>&lt;biblScope&gt;1&lt;/biblScope&gt;</code>

and replace the number one (1) with the page number you are transcribing. Which page should you transcribe? Select a page in the document that you find interesting.

Next, you will need to *very carefully* transcribe your page of text from the image into your document.  Make sure you do not make any changes to the text, even if you think they author has used poor grammar or misspelled a word.  You do not need to worry about line breaks but should start every new paragraph (or heading) with a <code>&lt;p&gt;</code> and end every paragraph (or heading) with a <code>&lt;/p&gt;</code>.

Once you have completed your transcription, look away from your computer for 30-45 seconds.  Staring into the distance every 10-20 minutes will keep your eyes from straining.  Also, shake out your hands at the wrists, to prevent repetitive stress injuries to your fingers.  

### Encoding Your Transcription ###

You are now going to *encode* or *mark-up* your text.  

Re-read your page and highlight / colour the following things:

+ Any persons mentioned (including any he/she if they refer to a specific person)
+ Any places mentioned
+ Any claims, assertions or arguments made
Now that you have highlighted these, you are going to put proper code around them.

For persons, surround your text with these

<code>&lt;persname key="Last, First" from="YYYY" to="YYYY" role="Occupation" ref="http://www.website.com/webpage.html"&gt; &lt;/person&gt;</code>

+ Inside the speech marks for **key**, include the real full name of the person mentioned 
+ In **from** and **to**, include their birth and death years, using ? for unknown years
+ In **role**, put the occupation, role or 'claim to fame' for this individual.  
+ In **ref**, put the URL (link) to the Dictionary of National Biography, Wikipedia or other biography website where you found this information. If there is a & in your link, you will need to replace this with &amp;amp;

 
For places, surround your text with these

<code>&lt;place key="Sheffield, United Kingdom" ref="http://tools.wmflabs.org/geohack/geohack.php?pagename=Sheffield&params=53_23_01_N_1_28_01_W_type:city_region:GB""&gt; &lt;/place&gt;</code>

+ In **key**, put the city and country with best information you can find for the modern names for this location
+ In **ref**, put a link to the relevant coordinates on Wikipedia GeoHack website (http://tools.wmflabs.org/geohack/)  (Where can you go to find coordinates in the first place? Why would we link to this particular site?)

For claims or arguments, surround your text with these

<code>&lt;interp key="reason" n="citation" cert="high" ref="http://www.website.com/webpage.html"&gt; &lt;/interp&gt;</code>

+ In **key**, explain why you believe this claim is true or not
+ In **n**, put a full citation to the relevant source
+ In **cert** (short for certainty), put: high, medium, low or unknown
+ In **ref**, put the link to the website where you got the information to assess this claim. If you are doing it based on lecture material, write "http://www.shu.ac.uk", but know that lecture material is a very weak source and should be avoided wherever possible

When you are happy with your work, hit save your work, give it a useful name, make sure it has .xml as the extension, and save it *and the .xsl file* to your repository.

### Viewing Your Encoded Text ###

To see your encoded text, make sure your .xml and .xsl file are in the same folder. Open your .xml file within a browser - you can do this by dragging the .xml file onto your browser.

If you now see a colour-coded version of your text, Congratulations!  

If you hover over the coloured sections, you should see a pop-up with the additional information you entered.  

### Now: transformations!

In the TEI Close Reading folder, there is a file called [SG_transformer.xsl](SG_transformer.xsl). Open that file in your text editor. What tags would it be looking for? What might it do to your markup? What line would you change in your XML file to get it to point to this stylesheet?

If the nature of your project will involve a lot of transcription, you would be well advised to use an XML editor like [OxygenXML](http://www.oxygenxml.com/), which has a free 1 month trial. The editor makes it easy to maintain _consistency_ in your markup, and also, to quickly create stylesheets for whatever purpose you need. There are also a number of utility programs freely available that will convert XML to CSV or other formats. One such may be [found here](https://code.google.com/p/xml2csv-conv/). But the best way to transform these XML files is with XSL. 
