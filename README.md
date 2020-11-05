# NLP Quick Start
## [See Notebook with Walkthrough](https://nbviewer.jupyter.org/github/IliaZenkov/NLP_basics_keras_nltk/blob/main/NLP_basics_keras_nltk.ipynb)

There are a wealth of fancy NLP algorithms available today - particularly using transformers, which has overshadowed a lot of the basics of NLP such as clustering and classification. However, simple algorithms are much easier to scale and often provide an excellent basis before building more complicated models.

I'm going to work through the Twitter Disasters dataset originally made available by Crowdflower. I found it available here.

## The Dataset
10,000 tweets are labeled as whether referring to a disaster (such as a flood, a pandemic) or not. I'm going to classify tweets as being truly related to disasters, or otherwise - memes and similar positive tweets often share words with negative tweets, so this may be somewhat challenging to get right.

Predicting relevance of tweets is an actionable problem if the data can be made available to first response teams such as police departments, hospitals, and mental health professionals.
