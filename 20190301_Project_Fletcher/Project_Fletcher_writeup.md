## Project Fletcher: tracking public sentiment towards marijuana over time

Natalia Lichagina

Presentation date: Mar 1st, 2019

### Summary of completed work

#### Problem statement

The goal was to analyze sentiment of New York Times articles about marijuana and how it changed over time.

#### Gathering data

Jupyter notebook: *Code_data_scraping.ipynb* 

I scraped NY Times search results (a total of 22K), which gave me:

- Newspaper section (only applicable for 1980 onwards) where the article was printed
- Headline
- Short description
- Date of publication

Even before I turned to my Jupyter notebook, I used Tableau to visually look at my data and validate the hypothesis that the sentiment on marijuana did change over time.  I manually created flags for crime, business, and health topics by looking for words like "arrest, sentence, seized" etc. with a series of if statements.  This quick and crude approach worked very well, getting me to very similar output that I later achieved with Python and 15+ hours of work.  My key takeaway from this was that the type of question I set out to answer was not sophisticated enough to require machine learning and in real life I would stop at this point.

#### Data processing

Jupyter notebook: *Code_analysis.ipynb* 

Tableau notebook (packaged with data): *Project_Fletcher_EDA.twbx*

In general, my data was pretty high quality. I just had to fix some of the inconsistent date formatting and, at later iterations, remove articles that had no text left after all the stop words were removed.

Although I had scraped 22K articles, ultimately I used only 4.6K for the analysis, leaving only those with direct mentions of marijuana/pot/weed/cannabis in the headline or summary text.  The reason was that NYT is very generous with its key words and so even articles with the most incidental mention of marijuana got displayed in the results.

To build the initial LDA model, I heavily relied on instructions from #source: https://www.machinelearningplus.com/nlp/topic-modeling-gensim-python/ - it was a great link! I did modify their algorithm a bit; their order of operations was: *remove stop words -> form bigrams -> lemmatize*.  I changed it to: *remove stop words -> lemmatize -> remove stop words -> form bigrams*.  I included an additional round of removing stop words because some of them showed up after the lemmatization was done AND I moved lemmatization ahead of bigram processing to increase the chance of catching commonly co-occuring word combinations.

#### Modeling

- I used **LDA, Guided LDA**, and **LSA + K-Means Clustering** for this project
- I leveraged the pyLDAvis visualization to check for quality of topics created by the LDA.  It's great, except that it takes a very long time to run. I wanted my LDA topics to be separate and interpretable, so I re-ran it at least 20 times with different combinations of topic number and random seed until I arrived at the three topics I could live with and ultimately used for the class presentation.
- Guided LDA took a bit of effort to get to work - specifically, it required additional data manipulation for the inputs. Ultimately, even though I was feeding it specific seed words, the output was still "bad" with same words showing up in different topics.  I also couldn't use the pyLDAvis with it, so it was harder to gauge how far the topics were separated from each other.
- I used LSA + K-Means mostly to learn how to use it, so I didn't try super hard to fix the terrible output it gave me on the first couple of tries. By "terrible" I mean that almost all articles ended up being grouped into one giant topic, with the rest getting assigned to 3 small topics that did not change much in frequency over the time period of analysis.  If I had used more combinations of topics and simply running different random seeds, as well as experimented with different clustering algorithms, I surely would've improved the result.  As is, since I had a good-enough output from LDA, I gave up rather quickly.
- All three models required very manual tuning with visual inspection for "is this good enough to stop?"  I guess this is the general feature of unsupervised learning especially when it comes to NLP, and I would consider it one of the weaknesses. I wish there was a way to automate the hyperparameter tuning.
- Lastly, making sense of the topics the unsupervised learning algorithms created was also a very hand-wavy process.  It helped to look at the "most representative" articles, but even then I applied my (admittedly biased) judgment for what felt representative enough to show to the class.

#### Conclusion

I do feel reasonably pleased the high level topics I've devised through a couple of the methods I used and, most importantly, with my conclusion that the public sentiment on marijuana DID change significantly in the past 50 years.  However, the high level of "setting my finger on the scale" that was needed in multiple steps of this process made me uncomfortable since it felt like I was forcibly proving what I believed to begin with.  (this is less of a complaint and more of a philosphical musing)
