# WEB APIS AND NLP: A REDDIT CLASSIFICATION CHALLENGE

# TABLE OF CONTENTS
<a id='table_of_contents'></a><br>
[Executive Summary](#section_1)<br>
[Problem Statement](#section_2)<br>
[Data Collection](#section_3)<br>
[Data Cleaning and EDA](#section_4)<br>
[Models and Conclusion](#section_5)<br>

<a id='section_1'></a>
# EXECUTIVE SUMMARY

Is there a fundamental difference between fans of rival teams?

If you're reading this, and you follow sports, you probably have a favourite team. That team most likely also has a rival and depending on how passionately you feel about your favourite team, there's a good chance you have quite a bit of disdain for that rival. After all, part of the oath is if you declare your love for one team, you have to declare your hatred for the other.

For some, this might seem arbitrary, to others, not so much.

Part of what underlies this all is the belief that there is a fundamental difference between you and them. 

If that's true, then it should be easy enough for a third party to separate rival supporters into their respective groups without them first declaring who they support.

This project is one such attempt to see if that is true.

This project contains two main aspects:
1. Reddit submission posts collected from r/LiverpoolFC and r/Everton using Pushshift's API.
2. The use of NLP to train models to see if they're able to accurately determine which posts came from r/LiverpoolFC and which came from r/Everton


<a id='section_2'></a>
# PROBLEM STATEMENT
[(Back to table of contents)](#table_of_contents)<br>

Is it possible to create a binary classification model that can accurately distinguish between posts in rival subreddits? 

Sports rivalries present us with the classic us vs them mentality. 

<a href="https://www.psychologytoday.com/ca/blog/fulfillment-any-age/201012/in-groups-out-groups-and-the-psychology-crowds"> It's a well-known principle in social psychology that people define themselves in terms of social groupings and are quick to denigrate others who don't fit into those groups. Others who share our particular qualities are our "ingroup," and those who do not are the "outgroup."</a>
    
    
Underlying all of this is the belief that there are irreconcilable differences between each side and that those differences are fundamental.

If that's true, then it should be simple enough for a NLP model to determine which reddit submissions come from one group and which reddit submissions come from another. 
    
In order to determine this, I have chosen two rival English Premier League clubs to see whether this is true.
    
    
    
I didn't however want to make this easy. If I had taken rivals from different parts of the country, the differences could be explained away by other things. To safeguard against that were Liverpool Football Club and Everton, two clubs from the same city and whose stadiums are one mile apart.
    
While I can't come to any definitive conclusions, I have decided to select a 90% accuracy rate as my target for whether this theory has any legs.
    
Why 90%?
    
Why not?


<a id='section_3'></a>
# DATA COLLECTION
[(Back to table of contents)](#table_of_contents)<br>

Using Pushshift's API and the Python's request library, 57, 908 Reddit submissions were collected. 29, 010 from r/LiverpoolFC. 28, 889 from r/Everton.
    
There was also around 60,000 comments collected, with an approximate equal distribution between the two sub-Reddits. But after initial modelling, they didn't prove to be as promising as the submissions data, so it was abandoned.


<a id='section_4'></a>
# DATA CLEANING AND EDA
[(Back to table of contents)](#table_of_contents)<br>

Outside of removing duplicate rows and imputing some values, initial data cleaning was minimal.

After subpar performance in my initial models however, I took a route and branch approach and engineered several variables in an attempt to improve the accuracy of my models.

These features were:
- tokens (tokenized forms of sentences)
- lems (lemmarized versions of tokens)
- porter_stems and snowball_stems (stemmed versions of tokens)
- word_count (count of total words)
- title_char_count (count of all characters in a title)
- sentence_count (count of total sentences)
- avg_word_length (average word length)
- avg_sentence_length (average sentence length)
- sentiment (a score giving to a piece of writing to determine whether it's positive, negative, or neutral)

<a id='section_5'></a>
# MODELS AND CONCLUSION
[(Back to table of contents)](#table_of_contents)<br>

|Model|Parameters|Training Score|Test Score|Precision|Recall|F1 Score|Specificity|
|:---|:---|:---|:---|:---|:---|:---|:---|
|**Logistic Regression**|TfidfVectorizer(max_features=12500, ngram_range=(1, 2), stop_words='english') , LogisticRegression(C=0.7)|91%|88%|87%|91%|88%|86%|
|**Random Forest**|TfidfVectorizer(max_features=12500, ngram_range=(1, 3), stop_words='english'), RandomForestClassifier(n_estimators=300, n_jobs=-1)|99%|88%|86%|90%|88%|85%|
|**SVC**|TfidfVectorizer(max_features=30000, stop_words='english'), SVC(C=0.7)|96%|86%|85%|87%|86%|85%|
|**KNN**|TfidfVectorizer(max_features=10000, ngram_range=(1, 3), stop_words='english'), KNeighborsClassifier(n_neighbors=1)|98%|70%|79%|55%|65%|85%|

<b>Note:</b> Due to time constraints, and initial poor performance, KNN was quickly abandoned.  

![Top r/LiverpoolFC features](https://i.imgur.com/phzUNWp.png)

![Top r/Everton features](https://i.imgur.com/wd86cCn.png)

As you can see from the table above, the four model types used all ended up overfitting.

While it can't be denied that overfit was an issue, what you can't see from the tables is the constraint my models were under; too much data.

While on the surface 57,000 observations may not seem like a lot of data, from the processing power I had available to me it proved to be incredibly difficult to hypertune the parameters of my models due to how long they would take to run.

Nonetheless, I'm quite happy with the results and I believe they speak to that rival sports fans aren't as different as they think they are.
