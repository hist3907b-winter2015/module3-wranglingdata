# A gentle introduction to Regular Expressions

_this text is adopted from the first drafts of_ The Macroscope _which is currently in-press with Imperial College Press. Users should consult that version once it's published._

## Introduction

A regular expression (also called regex) is a powerful tool for finding and manipulating text.  At its simplest, a regular expression is just a way of looking through texts to locate patterns. A regular expression can help you find every line that begins with a number, or every instance of an email address, or whenever a word is used even if there are slight variations in how it's spelled. As long as you can describe the pattern you're looking for, regular expressions can help you find it. Once you've found your patterns, they can then help you manipulate your text so that it fits just what you need. 

Regular expressions can look pretty complex, but once you know the basic syntax and vocabulary, simple ‘regexes’ will be easy. Regular expressions can often be used right inside the 'Find and Replace' box in many text and document editors, such as Notepad++ on Windows, or TextWrangler on OS X. Do not use Microsoft Word, however! To find these text editors, you can find [Notepad++ here](http://notepad-plus-plus.org/) or [TextWrangler here](http://www.barebones.com/products/textwrangler/).

+ NB in Notepad++ when you do a search, to use regular expressions in your search you must tick off the checkbox enabling them. Otherwise, Notepad++ will treat your search literally, looking for that exact _text_ rather than the _pattern_. Similarly in Textwrangler, you need to tick off the box marked 'grep' when you bring up the search dialogue panel.

## Some basic principles

> protip: there are libraries of regular expressions, online. For example, if you want to find all postal codes, you can search “regular expression Canadian postal code” and learn what ‘formula’ to search for to find them

Let's say you're looking for all the instances of "cat" or "dog" in your document. When you type the vertical bar on your keyboard (it looks like ```|```, shift+backslash on windows keyboards), that means 'or' in regular expressions. So, if your query is dog|cat and you press 'find', it will show you the first time either dog or cat appears in your text.

If you want to replace every instance of either "cat" or "dog" in your document with the world "animal", you would open your find-and-replace box, put ```dog|cat``` in the search query, put animal in the 'replace' box, hit 'replace all', and watch your entire document fill up with references to animals instead of dogs and cats.

The astute reader will have noticed a problem with the instructions above; simply replacing every instance of "dog" or "cat" with "animal" is bound to create problems. Simple searches don't differentiate between letters and spaces, so every time "cat" or "dog" appear within words, they'll also be replaced with "animal". "catch" will become "animalch"; "dogma" will become "animalma"; "certificate" will become "certifianimale". In this case, the solution appears simple; put a space before and after your search query, so now it reads: 

```dog | cat```  

With the spaces, "animal" replace "dog" or "cat" only in those instances where they're definitely complete words; that is, when they're separated by spaces.

The even more astute reader will notice that this still does not solve our problem of replacing every instance of "dog" or "cat". What if the word comes at the beginning of a line, so it is not in front of a space? What if the word is at the end of a sentence or a clause, and thus followed by a punctuation? Luckily, in the language of regex, you can represent the beginning or end of a word using special characters. 

``` \< ``` 

means the beginning of a word. In some programs, like TextWrangler, this is used instead:

``` \b ```

so if you search for ```\<cat``` , (or, in TextWrangler, ```\bcat``` )it will find "cat", "catch", and "catsup", but not "copycat", because your query searched for words beginning with "cat". For patterns at the end of the line, you would use:

``` \> ``` 

or in TextWrangler,

``` \b ```

again.  The remainder of this walk-through imagines that you are using Notepad++, but if you’re using Textwrangler, keep this quirk in mind. If you search for 

``` cat\> ``` 

it will find "cat" and "copycat", but not "catch," because your query searched for words ending with -"cat".

Regular expressions can be mixed, so if you wanted to find words only matching "cat", no matter where in the sentence, you'd search for 

``` \<cat\> ```

which would find every instance. And, because all regular expressions can be mixed, if you searched for (in Notepad++; what would you change, if you were using TextWrangler?)

``` \<cat|dog\> ```

and replaced all with "animal", you would have a document that replaced all instances of "dog" or "cat" with "animal", no matter where in the sentence they appear. You can also search for variations within a single word using parentheses. For example if you were looking for instances of "gray" or "grey", instead of the search query

``` gray|grey ```

you could type 

``` gr(a|e)y ```
 
instead. The parentheses signify a group, and like the order of operations in arithmetic, regular expressions read the parentheses before anything else. Similarly, if you wanted to find instances of either "that dog" or "that cat", you would search for: 

``` (that dog)|(that cat) ```

 Notice that the vertical bar | can appear either inside or outside the parentheses, depending on what you want to search for.

The period character . in regular expressions directs the search to just find any character at all. For example, if we searched for:

``` d.g ``` 

the search would return "dig", "dog", "dug", and so forth. 

Another special character from our cheat sheet, the plus + instructs the program to find any number of the previous character. If we search for 

``` do+g ```

it would return any words that looked like "dog", "doog", "dooog", and so forth. Adding parentheses before the plus would make a search for repetitions of whatever is in the parentheses, for example querying 

``` (do)+g ```

would return "dog", "dodog", "dododog", and so forth.

Combining the plus '+' and period '.' characters can be particularly powerful in regular expressions, instructing the program to find any amount of any characters within your search. A search for 

``` d.+g ```

for example, might return "dried fruits are g", because the string begins with "d" and ends with "g", and has various characters in the middle. Searching for simply ".+" will yield query results that are entire lines of text, because you are searching for any character, and any amount of them.

Parentheses in regular expressions are also very useful when replacing text. The text within a regular expression forms what's called a group, and the software you use to search remembers which groups you queried in order of their appearance. For example, if you search for 

``` (dogs)( and )(cats) ```

which would find all instances of "dogs and cats" in your document, your program would remember "dogs" is group 1, " and " is group 2, and "cats" is group 3. Notepad++ remembers them as "\1", "\2", and "\3" for each group respectively.

If you wanted to switch the order of "dogs" and "cats" every time the phrase "dogs and cats" appeared in your document, you would type 

``` (dogs)( and )(cats) ```

in the 'find' box, and 

``` \3\2\1 ```

in the 'replace' box. That would replace the entire string with group 3 ("cats") in the first spot, group 2 (" and ") in the second spot, and group 1 ("dogs") in the last spot, thus changing the result to "cats and dogs".

The vocabulary of regular expressions is pretty large, but there are many cheat sheets for regex online (one that we sometimes use is http://regexlib.com/CheatSheet.aspx. Another good one is at http://docs.activestate.com/komodo/4.4/regex-intro.html)

