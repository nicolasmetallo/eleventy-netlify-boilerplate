---
title: Text Analysis with Python and SpaCy
date: 2018-09-04T04:49:34.619Z
summary: >-
  Use Google Colab to analyze the latest speeches from Mauricio Macri, the
  president of Argentina, with SpaCy and Python.
tags:
  - nlp
  - sentiment analysis
  - speech
  - mauricio macri
---
You can find the related code in [my repository](https://github.com/nicolasmetallo/simple-nlp-analysis-with-spacy/).

SpaCy is a library for Python that's been written from the ground up in Cython, making it really fast. It's also a great way to prepare text for deep learning as it works with TensorFlow, PyTorch, scikit-learn, and more. You can learn more, and play with many examples, at their [home page](https://spacy.io/).

First, clone the repository

```
!git clone https://github.com/nicolasmetallo/simple-nlp-analysis-with-spacy.git
```

And then install all pre-requisites

```
!pip install -r requirements.txt
```

### Install SpaCy and misc

```
!pip install spacy
!python -m spacy download es_core_news_sm
```
### Read and process President's Macri Speeches
```
import pandas as pd
import spacy
from collections import Counter

# Load spanish tokenizer, tagger, parser, NER, and word vectors
nlp = spacy.load("es_core_news_sm")

# Read and process txt docs
march_speech = open('discurso-macri-marzo.txt', 'r', encoding='utf-8').read()
march_doc = nlp(march_speech)

september_speech = open('discurso-macri-septiembre.txt', 'r', encoding='utf-8').read()
september_doc = nlp(september_speech)
```
### Sentence Tokenization & Word-Cloud
```
def sentence_tokenization(doc_name):
  for num,sentence in enumerate(doc_name.sents):
    print('{0}: {1}'.format(num,sentence))

sentence_tokenization(march_doc)
```
```
# !pip install wordcloud
from wordcloud import WordCloud
import matplotlib.pyplot as plt

def print_wordcloud(doc_name):
  base = [ token.text for token in doc_name if token.is_stop != True and token.is_punct !=True ]
  wordcloud = WordCloud(width=600, height=300, 
                      max_font_size=150).generate(str(base))
  plt.figure(figsize=(15, 12))
  plt.imshow(wordcloud, interpolation="bilinear")
  plt.axis('off')
  plt.show()

print_wordcloud(march_doc)
```
```
sentence_tokenization(september_doc)
print_wordcloud(september_doc)
```
### Count Most Common Verbs, Nouns
```
# Count verbs, nouns with stopwords and remove punctuation, stops.
def count_most_common(doc_name, verb_or_noun):
  base = [ token.text for token in doc_name if token.is_stop != True and token.is_punct !=True and token.pos_ == verb_or_noun ]
  word_freq = Counter(base)
  most_common = word_freq.most_common(10)
  
  return most_common
  
march_most_common_nouns = count_most_common(march_doc, verb_or_noun = 'NOUN')
march_most_common_verbs = count_most_common(march_doc, verb_or_noun = 'VERB')

september_most_common_nouns = count_most_common(september_doc, verb_or_noun = 'NOUN')
september_most_common_verbs = count_most_common(september_doc, verb_or_noun = 'VERB')
```
```
data_dict = {'march_most_common_nouns':march_most_common_nouns, 'march_most_common_verbs':march_most_common_verbs,
            'september_most_common_nouns':september_most_common_nouns, 'september_most_common_verbs':september_most_common_verbs}

data_nouns_verbs = pd.DataFrame(data_dict)
data_nouns_verbs
```
### NLP with SpaCy – Sentiment Analysis
```
# adding extensions
from spacy.tokens import Doc

#!pip install nltk
import nltk
nltk.download('vader_lexicon')

#!pip install twython

from nltk.sentiment.vader import SentimentIntensityAnalyzer
sent_analyzer = SentimentIntensityAnalyzer()

# download and load english language into spacy
# !python -m spacy download en
```
```
def sentiment_scores(doc_name):
    return sent_analyzer.polarity_scores(doc_name.text)

Doc.set_extension("sentiment_analyzer",getter=sentiment_scores)
```
```
def sentiment_analysis_on_txt_file(input_txt, lang = 'en'):
  input_text = open(input_txt, 'r', encoding='utf-8').read()
  if lang is 'es':
    nlp = spacy.load("es_core_news_sm")
  else:
    nlp = spacy.load('en')
  input_doc = nlp(input_text)
  f = input_doc._.sentiment_analyzer
  print(input_txt)
  for k,v in f.items():
    print(k,v)
    
sentiment_analysis_on_txt_file('discurso-macri-marzo-english.txt')
print('\n')
sentiment_analysis_on_txt_file('discurso-macri-septiembre-english.txt')
```
