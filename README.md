# Problem Statement
The COVID-19 governmental response has been largely regional and state-based in nature. Some states have enacted strictly-enforced stay-at-home policies, while others have provided guidelines. It would be worthwhile to compare the sentiment analysis of social media posts across geographic regions and compare them to both the local policies on social distancing and the occurrences of the pandemic in those areas.

**KEY QUESTIONS:**
- What does the sentiment about COVID-19 across time look like for New York City, NY vs. Houston, TX?
- Do changes in sentiment align with changes in policy?
- Are there any keywords that demonstrate a meaningful story with regard to the sentiment of the populace in each location?

**LIMITED GOAL OF PROJECT**

We seek to provide a proof of concept that social media can indeed be used as a source for sentiment analysis via Natural Language Processing, and more specifically, that reactions to COVID-19 responses show differing sentiment in different locations. While Twitter may be a more preferable source for analysis, barriers to its use led us to work with Reddit data, which we show is still a viable option.

# Executive Summary
Tasked with trying to mine social media data to analyze sentiment differences regarding COVID-19 responses across different geographies, we created a process for sourcing Reddit data and using VADER sentiment analysis to track and compare sentiment over time. We align the sentiment data with key governmental policy changes or signposts.

Our program can sample data from any subreddit. As a proxy for geographic data, we sampled from general discussion subreddits for localities and decided on a direct contrast and comparison of New York City, NY and Houston, TX.

Criteria for choosing these locations:
1. A contrasting element regarding how COVID-19 was affecting the area, along with a differing timing and level of governmental response
2. A minimum amount of data to work with

Sentiment analysis data needs to be interpreted with care. Sentiment analysis only tells you **what** the current mood is, *not* **why** it is that way. Positive and negative sentiments can cancel each other out in the aggregate, even if the intensity of sentiment is extreme, if there are roughly equal polarized reactions to a post or an event.

The resulting story was clearest when focusing on posts mentioning each of the respective state governors, where Houstonians tended to bristle at any suggestion of lockdown restrictions and react very positively to their lifting, while New Yorkers seemed more generally accepting of stricter policies given the high incidence of COVID-19 in the five boroughs of New York City.

# Table of Contents
## Python Notebooks
- [0 Fetch NY Times Covid Data](./code/0-Fetch-NYTimes.ipynb)
- [1 Fetch Twitter](./code/1-Fetch-Twitter.ipynb) - *not used for analysis*
- [2 Fetch Reddit](./code/2-Fetch-Reddit.ipynb)
- [3 Analyze Sentiment](./code/3-Analyze-sentiment.ipynb)
- [Shortcut to Saved Graphs](./graphs/)

## Tableau Files
- [Workbook](./tableau/Covid.twb)
- [Data](.data/NYTimes-counties-2020-05-07.csv.bz2)
- [Data With Estimates](.data/NYTimes-counties-with-estimates-2020-05-07.csv.bz2)

## Presentation
- [Presentation](./presentation/Covid%2019%20Sentiment%20Analysis.pdf)

# Data Acquisition, Ingestion, & Cleaning
The Reddit API has two databases for original posts (submissions) and comments, respectively.

We chose to focus on comments, as we felt they were more likely to express sentiment. Submissions were needed to
ensure topic relevance.

Our solution was to build a custom two-step data gathering module that would utilize the information in both databases, and pass the resulting texts into the the VADER sentiment analyzer (Hutto & Gilbert 2014).

*Our data gathering module...*
- ...first searches the submissions database for the most commented posts using a keyword or set
of keywords in a given time frame (for our baseline, the keywords we used were “covid-19”, “coronavirus”, “quarantine”,
and “pandemic” for the 80 day period between February 2nd and May 10th) inside given subreddits (the local subreddits for
Houston and NYC)…

- … extracts the unique link ids of those posts…

- … then performs a query on the comments database to pull all comments relating to those link ids (i.e. all comments for
those posts) .

The data for each subreddit were then balanced by taking the smallest sample (Houston had fewer comments than New
York) and then sampling all subreddits for that number of data points to be fed into the sentiment analyzer).

*The VADER analyzer...*  
- ...matches 7500 human-annotated keywords for polarity (postive or negative) and intensity (how strongly felt).  

- ...incorporates contextual information such as negations ('not'), intensifiers ('very'), hedges ('a little...'), and even emoticons (':)')

- ...works on unprocessed text, so no need to tokenize, etc.

# Software Requirements
- Python packages
  - Pandas
  - Numpy
  - Matplotlib
  - datetime
  - time
  - requests
  - Beautiful Soup
  - nltk.sentiment.vader SentimentIntensityAnalyzer
  - Tweepy
- Tableau (for visualizing COVID-19 case data)

# Contributors
The data scientists responsible for this project include:
- Alex Klapheke
- Jon Godin
- Luken Weaver
- Reza Farrokhi Saray
