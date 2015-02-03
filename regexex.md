# Introduction

The correspondence of the Republic of Texas was collated into a single volume and published with a helpful index in 1911. It was scanned and OCR'd by Google, and is now available as a text file from the Internet Archive. You can see the OCR'd text at [this page](http://archive.org/stream/diplomaticcorre33statgoog/diplomaticcorre33statgoog_djvu.txt). We are going to grab the index from that file, and transform it using regex. 

There are several hundred entries in that index. You could clean them up by hand, deleting and cutting and pasting, but with the power of regex, we'll go from this:

``` Sam Houston to A. B. Roman, September 12, 1842 101 

Sam Houston to A. B. Roman, October 29, 1842 101 

Correspondence for 1843-1846 — 

Isaac Van Zandt to Anson Jones, January 11, 1843 103 ```

...to nicely CSV-formatted table like this:

``` Sam Houston, A. B. Roman, September 12 1842 

Sam Houston, A. B. Roman, October 29 1842 

Isaac Van Zandt, Anson Jones, January 11 1843 ```

## Getting started

In the previous module, we learned how to automatically grab text from sites like the Internet Archive. In this particular exercise today, we'll just copy and paste from the OCR'd text page (as we're working with a single document).

+ Copy the OCR'd text from this file: http://archive.org/stream/diplomaticcorre33statgoog/diplomaticcorre33statgoog_djvu.txt

and paste it into a new Notepad++ or Textwrangler file. Save it as 'texan-correspondence.txt'. Remember to save a spare copy of your file before you begin - this is very important, because you're going to make mistakes that you won't be sure how to fix. 

+ Delete everything in your working copy of the file except for the index of the list of letters. 

That is, you’re looking for the table of letters, starting with ‘Sam Houston to J. Pinckney Henderson, December 31, 1836 51’ and ending with ‘Wm. Henry Daingerfield to Ebenezer Allen, February 2, 1846 1582’. Your file will now have approximately 2000 lines in it.

Notice that there is a lot of text that we are not interested in at the moment: page numbers, headers, footers, or categories. We're going to use regular expressions to get rid of them. What we want to end up with is a spreadsheet that is arranged in three columns:

``` Sender, Recipient, Date ```

Scroll down through the text; notice there are many lines which don't include a letter, because they're either header info, or blank, or some other extraneous text. We're going to get rid of all of those lines. We want to keep every line that has this information in it: Sender to Recipient, Month, Date, Year, Page

## The workflow
We start by finding every line that looks like a reference to a letter, and put a tilde (a ~ symbol) at the beginning of it so we know to save it for later. Next, we get rid of all the lines that don't start with tildes, so that we're left with only the relevant text. After this is done, we format the remaining text by putting commas in appropriate places, so we can import it into a spreadsheet and do further edits there.

### Step One

*Identifying Lines that have Correspondence Senders and Receivers in them*

_Discussion_ Read in full before doing any manipulation of your text!

In Notepad++, press ctrl-f or search->find to open the find dialogue box.  In that box, go to the 'Replace' tab, and check the radio box for 'Regular expression' at the bottom of the search box. In TextWrangler, hit command+f to open the find and replace dialogue box. Tick off the ‘grep’ radio button (which tells TextWrangler that we want to do a regex search) and the ‘wraparound’ button (which tells TextWrangler to search everywhere).

Remember from our basic introduction that there's a way to see if the word "to" appears in full. Type

``` \<to\> ```

in the search bar.  Recall in TextWrangler we would look for ``` \bto\b ``` instead. This will find every instance of the word "to" (and not, for instance, also ‘potato’ or ‘tomorrow’).  

We don't just want to find "to", but the entire line that contains it. We assume that every line that contains the word “to” in full is a line that has relevant letter information, and every line that does not is one we do not need. You learned earlier that the query ``` .+ ``` returns any amount of text, no matter what it says. Assuming you are using Notepad++, if your query is 

``` .+\<to\>.+ ``` 

your search will return every line which includes the word "to" in full, no matter what comes before or after it, and none of the lines which don't. In TextWrangler, your query would be ``` .+\bto\b.+. ```` 

As mentioned earlier, we want to add a tilde ~ before each of the lines that look like letters, so we can save them for later. This involves the find-and-replace function, and a query identical to the one before, but with parentheses around it, so it looks like 

``` (.+\<to\>) ```

and the entire line is placed within a parenthetical group. In the 'replace' box, enter 

``` ~\1 ```

which just means replace the line with itself (group 1), placing a tilde before it. 

+ Copy and past some of your text into [RegExr.com]. Write your regular expression (ie, what you're trying to find), and your substitution (ie, what you're replace with) in the RegExr interface. Once you're satisfied that you've got it right, run this on your working copy of your text. You might want to save with a new name at each step of this process, so that if something goes wrong, you can recover easily!

### Step Two

*Removing Lines that Aren’t Relevant*

_Discussion_

After running the find-and-replace, you should note your document now has most of the lines with tildes in front of it, and a few which do not. The next step is to remove all the lines that do not include a tilde. The search string to find all lines which don't begin with tildes is 
``` \n[^~].+ ```

A ``` \n ``` at the beginning of a query searches for a new line, which means it's going to start searching at the first character of each new line.  *However, given the evolution of computing, it may well be that this won’t quite work on your system*. Linux based systems use ``` \n ``` for a new line, while Windows often uses ``` \r\n ```, and older Macs just use ``` \r ````. These are the sorts of things that digital historians need to keep in mind! Since this will likely cause much frustration, your safest bet will be to save a copy of what you are working on, and then experiment to see what gives you the best result. In most cases, this will be:

``` \r\n[^~].+ ```

Within a set of square brackets ``` [] ``` the carrot ``` ^ ``` means search for anything that isn't within these brackets; in this case, the tilde ``` ~ ```. The  ```.+ ``` as before means search for all the rest of the characters in the line as well. All together, the query returns any full line which does not begin with a tilde; that is, the lines we did not mark as looking like letters. 

By finding all ``` \r\n[^~].+ ```and replacing it with nothing, you effectively delete all the lines that don't look like letters. What you're left with is a series of letters, and a series of blank lines. 

+ Devise the regular expression that works on your machine, and replace with nothing. Test on RegExr or on a smaller chunk of your text to make sure it works.

### Step Three

*Removing the Blank Lines*

_Discussion_

Given what we know about new lines, what do you think a regex would be for a blank line? Try that out in RegExr now. In Notepad++, it'll most likely be ``` \n\r ``` ; in textwrangler ``` ^\r ```. Do you see what the difference is? Examine the cheat sheets and documentation at RegExr and make a note in your notebook on the difference.

+ Devise the regular expression to find the blank lines, and replace with nothing. Are you still saving at each successful step?

### Step Four

*Transforming into CSV format*

_Discussion_

To turn this text file into a spreadsheet, we'll want to separate it out into one column for sender, one for recipient, and one for date, each separated by a single comma. Notice that most lines have extraneous page numbers attached to them; we can get rid of those with regular expressions. There's also usually a comma separating the month-date and the year, which we'll get rid of as well. In the end, the first line should go from looking like:

``` ~Sam Houston to J. Pinckney Henderson, December 31, 1836 51 ```

to

``` Sam Houston, J. Pinckney Henderson, December 31 1836 ```

such that each data point is in its own column.

You will start by removing the page number after the year and the comma between the year and the month-date. To do this, first locate the year on each line by using the regex:

``` [0-9]{4} ```

As a cheat sheet shows, ```[0-9] ``` finds any digit between 0 and 9, and ``` {4} ```will find four of them together. Now extend that search out by appending ``` .+ ``` to the end of the query; as seen before, it will capture the entire rest of the line. The query 

``` [0-9]{4}.+ ```

will return, for example, "1836 51", "1839 52", and "1839 53" from the first three lines of the text. We also want to capture the comma preceding the year, so add a comma and a space before the query, resulting in 

``` , [0-9]{4}.+ ```

which will return ", 1836 51", ", 1839 52", etc.	

The next step is making the parenthetical groups which will be used to remove parts of the text with find-and-replace. In this case, we want to remove the comma and everything after year, but not the year or the space before it. Thus our query will look like:

``` (,)( [0-9]{4})(.+) ```

with the comma as the first group "\1", the space and the year as the second "\2", and the rest of the line as the third "\3".  Given that all we care about retaining is the second group (we want to keep the year, but not the comma or the page number), what will the *replace* look like?

+ Find the dates using a regex, and replace so that only the *second* group in the expression is kept. You might want to consult the introduction to regex again before you execute this one.

### Step Five

*Removing the tildes*

+ Find the tildes that we used to mark off our text of interest, and replace them with nothing to delete them.

### Step Six

*Separating Senders and Receivers*

_Discussion_

Finally, to separate the sender and recipient by a comma, we find all instances of the word "to" and replace it with a comma. Although we used ``` \< ``` and ``` \> ```(in TextWrangler, ``` \b ```) to denote the beginning and end of a word earlier in the lesson, we don't exactly do that here. We include the space preceding “to” in the regular expression, as well as the ``` \> ``` (``` \b ``` in TextWrangler) to denote the word ending. Once we find instances of the word and the space preceding it, ``` to\> ``` (nb, remember the space!!!) we replace it with a comma ``` , ```.
 
+ Devise the regex to find the word, and replace with a comma.

### Step Seven

*Cleaning up*

_Discussion_

You may notice that some lines still do not fit our criteria. Line 22, for example, reads "Abner S. Lipscomb, James Hamilton and A. T. Bumley, AugUHt 15, ". It has an incomplete date; these we don't need to worry about for our purposes. More worrisome are lines, like 61 "Copy and summary of instructions United States Department of State, " which include none of the information we want. We can get rid of these lines later in Excel. 	

The only non-standard lines we need to worry about with regular expressions are the ones with more than 2 commas, like line 178, "A. J. Donelson, Secretary of State [Allen,. arf interim], December 10 1844". Notice that our second column, the name of the recipient, has a comma inside of it. If you were to import this directly into Excel, you would get four columns, one for sender, two for recipient, and one for date, which would break any analysis you would then like to run. Unfortunately these lines need to be fixed by hand, but happily regular expressions make finding them easy. The query:

``` .+,.+,.+, ```

will show you every line with more than 2 commas, because it finds any line that has any set of characters, then a comma, then any other set, then another comma, and so forth.

+ Devise a regex to find these lines with more than 2 commas in them. Fix these lines manually, and then click 'find next' to grab the next one.

+ At the top of the file, add a new line that simply reads "Sender, Recipient, Date". These will be the column headers. Save as 'cleaned-correspondence.csv'.

*Congratulations!*

You've now used regex to extract, transform, and clean historical text. As a csv file, you could now load this data into a network analysis program such as [Gephi](http://gephi.org) to explore the ramifications of this correspondence network. Upload your file to your repository, and make a note of the original location of the file, the transformations that you've done, and the date/time. You will be using your 'cleaned-correspondence.csv' file in the next exercise using *Open Refine*, where we'll sort out some of the messy OCR (fixing names, and so on). 