### 2. Details of the MCQBot
   This is the implementation of a simple bot to answer multiple choice question. The idea is pretty simple - design a function that    takes as input a question, and a list of choices (number of choices is not fixed), and that returns the index of the choice believed to be the right/ most popular one. 
   
```
Do you feel like visiting the seaside?
1.) Never
2.) Occassionally but only when forced to
3.) I'd like to.
4.) The sea is my one true passion. Dry land is death
```
#### How the data scraper Works   
   This particular bot focusses on semantic analysis for NLP to answer pseudo open field questions. It looks through google results and
   uses the information it receives to make the best choice.
   This is taken from an open source API on github. There is a simple API that lets you make some research in google, which has been wrapped in python, [Google-Search-API](https://github.com/abenassi/Google-Search-API). 
   Here's the syntax of the python statements I've used to search for stuff on Google.
    
```
from google import google
my_search = google.search('Search terms')
```
   This Google API software uses screen scraping to retreive search results from google.com. 
   
   **my_search** will contain a list of GoogleResult objects. To this, I **add the name of the user so as to search specifically within their social media, their postings etc.**
   
```
GoogleResult:
self.name # The title of the link
self.link # The link url
self.description # The description of the link
self.thumb # The link to a thumbnail of the website (not implemented yet)
self.cached # A link to the cached version of the page
self.page # What page this result was on (When searching more than one page)
self.index # What index on this page it was on
```   
After cleaning the text extracted from the search, I concatenate all the text together as one really long string that represents all the text available on first pages of google. I then score each of my search results in order to determine which one would be the best. 

#### How the NLP syntax analysis Works
In order to find the answer, I use n-grams. For example, if the search term is 'How often visit sea FName LName', then,
```
1-grams = ["How", "often", "visit", "sea", "FName", "LName"]
2-grams = ["How often", "visit sea",  "FName LName"]
etc.
```

Rather that counting in the google search result the entire choice string (preprocessed), we look for the 1-grams, 2-grams and the full string (preprocessed as method 1).
The score is thus the sum of these sub scores, each sub score having a multiplier:

```
total_score = 1*(1-grams occurences) + 3*(2-grams occurences) + 10*(full string occurences)
```

In that way, if only one word of the choice occurs, it only adds 1 point to the total score, if a 2-gram is found is found, adds 3 points, and if the full string is found, it adds 10 points. 

#### Results of the MCQBot
 The counter measures the number of entries (i,e the one with the highest score) and chooses the relevant answer. In the case of Kylie Jenner, the answer would be 3 since her social media contains multiple references to the seaside.
 This would select the cuisine and then add in food from the restaurant and add it into the checkout using the same principles.   
