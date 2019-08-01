---
layout: post
title: spaCy v Gensim
date: 2019-07-31 00:00:00 +0800
description: NLP techniques # Add post description (optional)
img: spacy.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Enron, NLP, SpaCy, Gensim] # add tag
---

## What makes a language?
Britannica defines language as a way that participants in its culture/group, express themselves. At its core syntax, grammar, semantics apply often times very differently from culture to culture, how then do we interpret speech or words? How would computers, with no 'knowledge' of any language interpret or infer meaning?

## Word Embeddings?
Word embedding is a way in which computers understand paragraphs and sentences. One can imagine this as a dictionary. As we look up dictionaries for the meaning of words, so do computers. Computers would look up a vector space ('dictionary') infer meaning from associated words. There are two main models around how words are perceived (which we would cover while we go through later under NMF and LDA). This process is called *vectorization* (or, to create word embeddings) and there are 4 key processes to arrive at our final vector space.

1) **Tokenization**
  The process of splitting sentences into words

2) **N-grams**
  The process of grouping words together as they hold meaning together, not apart. e.g. New York.

3) **Implement stop words**
  The process of removing words that are 'meaningless'/do not add value to understanding speech, e.g. purely grammatical purposes.

4) **Lemmatization/Stemming**
  The process of standardizing the same word despite its many forms, e.g. run, running, ran. -> run

The result of undergoing the above is a vector that a machine has learned - a mapped set of words or phrases to vectors of numerical values.

## Packages/Libraries
Before we get into the workflow though, we would have to decide between 2 powerful libraries, spaCy or Gensim. As we are clearly at the vectorization stage the focus here is to structure our word embeddings in a way that has would help us boost the signal (aka produce clearer/more meaningful results).

Some people may ask about the order in which preprocessing is being performed. particularly with the steps of applying stop words and n-gram application.


There are certain weaknesses in each package that the other has powerful tools for. so... why not use both? yes, we can.

------------------------------

1) Tokenization

  The initial step of tokenization is not significant, we can simply use a lambda function of a .split() or NLTK to help us for that, so I cannot attribute any winner for this part of the process.  

2) N-grams

  SpaCy does not seem to support n-grams, and while we may use NLTK to create bigrams, this is done en-masse with a (n,n+1), (n+1,n+2)... series being generated. This does not necessarily mean anything as we appear to be throwing a process without any meaningful application of the creation of n-grams. Gensim does this differently and reviews each chunk (paragraph, email, sentence, depending on how it is set up.) only to infer n-grams through the frequency of words occurring together as outlined in this [paper](https://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf).

  Gensim in my view is a more nuanced approach and while some n-grams may slip through, it is personally more tolerable as compared to brutal attribution like NLTK.

3) Implement stop words.

  I am sure all of us have at one point in our lives communicated(or tried to) with someone who spoke a different language. Is it any wonder why for more simplistic sentences we were able to understand them? or despite a poor grasp of grammar, some how ideas were still communicated? hopefully you would wonder no further as it is because key words were mentioned and the concept was understood beyond your heavy gesticulation.

    * These words are what we call stop words - words that are commonly used but have no real topical meaning.

Some of these words include I, we, be, really, them, they, and, the, are, for, at. let us use the preceding sentence as an example.

    * words call stop words - words commonly used no real topical meaning.

or how about:

    * process standardizing same word despite many forms


pretty sure we can guess what that sentence refers to.

SpaCy? or Gensim you ask? well, if you're unwilling to download over 1 GB of data for use spaCy may be the better deal. SpaCy offers several packages [here](https://spacy.io/models/en) and even for different langauges. Both libraries allow for customization of these words on the fly, which i would thoroughly recommend, as we would need to filter out 'words' that may not mean much depending on what sort of signal you may be looking for.

here are the additional stop words that I have used:
```python
custom_stop_words = ['from', 'subject', 're', 'edu', 'use','fwd','please',
                   'thanks','enron','inc','copy','mail','know','need',
                   're','yo','said','also','email','www','may','cell','fax',
                   'http','br','table','width','href','ahref','align','would',
                   'font','go','width_height','tr','nbsp','face_arial','gif','the',
                    'align_center', 'td_td','table_width','colspan_nbsp','colspan_align',
                    'like','courier_nbsp','ueta_ucita','hmtf']
```

4) Lemmatization/Stemming
```
This is an example of Lemmatize.
This is an example of Stemm.
```

Both libraries offer processes to lemmatize and stem, and they are not significant enough to warrant such a thorough examination of which to use. I have stuck with spaCy's lemmatization for consistency. In the process of lemmatizing, I have also utilized its [pipeline](https://spacy.io/usage/processing-pipelines) process in which it performs a part of speech (POS) tagging. This is especially useful if the NLP experiment you are about to perform would like to include a level of named entity recognition (NER) and for noise reduction. After all, pronouns are meaningless.

## Finally...
Are we done yet?

I guess.. This process is particularly long and arduous, not only is the processing time long (since we have hundreds of thousands of entries.), but reiteration of the process is a **MUST**. The result of running stop words for instance, requires a post mortem review to see if any additional words that have been created (as shown above) should have been included. For instance, I would not want html results in my message while it *may* be the case.

The steps we have done so are considered some what rudimentary, a better system could implement the use of Doc2Vec where a shallow neural network is applied and a step up from Word2Vec (both found in Gensim) the vectorization as such is done on both a word and document level. There are ample resources online to illustrate use, but while implementing Doc2Vec for this exercise it was met with vector compatibility issues wherein the algorithm appears to require class level edits.

In the next post, we would go through some modelling techniques to uncover some hidden topics in our corpus.
