---
layout: post
title: Uncovering the Corpus
date: 2019-07-30 00:00:00 +0800
description: Getting the Enron Corpus and making it machine readable # Add post description (optional)
img: how-to-start.jpg # Add image post (optional)
tags: [Enron, Data Cleaning, Regex, Enron] # add tag
---
## 500,000 entries, 2,000,000 data points

If you have not(yet) been able to find the corpus, here's a helpful [link](https://www.cs.cmu.edu/~enron/) with over 500,000 entries and 1.4 GB worth of text files to plough through, this dataset is indeed quite a behemoth to wrangle. In its raw form, you would need to create some command line loops to extract each file. This process may take a while, ideally you would like to also store this as a .json or .csv for future use as he file size is slightly smaller, about 30% it seems.

![cmd scrape]({{site.baseurl}}/assets/img/cmdpt.jpg)

## Anatomy of an Email.
An Email comprises of several components.
1. From
2. To
3. Cc
4. Bcc
5. Subject
6. Body
7. Date

However, it is worth noting that in a closed loop (where all participants are accounted for), there really are just 5 items. From, Subject, Body, Date, User. This is because for any message, 2 messages are created. One on part of the outbox, the other, inbox. We have applied this concept to our processing and have extracted the 4 and included the direction of the message (received/sent)

## Hello Regex.
In my years as a data analyst, regex has never been so in demand yet easily dismissed. When it comes to languages people simply dismiss it on grounds of complexity or high level knowledge - to which I can now safely say, no. Understanding regex allows for significant enhancements to your NLP experiments and refinement to simplistic if/else statements when it comes to legacy programs. I would recommend trying out your expressions [online](https://regexr.com). As they say practice makes perfect and soon you'll be able to handle any form of regex from words, to emails and even code (which you would need to some extent in this exercise.)

## Sculpting.
Data cleaning is such a prerogative term. Yes, while we are cleansing it, we are also showing data as it 'should be'. To that end, isn't sculpting more appropriate?

Opinions aside, working through pre-processing requires a number of iterations and reviewing results, there is no easy way around this and revisiting even at the pre-processing stage is just as likely to occur when we are building our models. In my view only through experience over time or an in depth knowledge of the datasets can we mitigate this exercise. This [link](http://www.cs.rpi.edu/~goldberg/publications/cleaning.pdf) provides some insight into dealing with issues but I personally felt that some are a little convoluted and have instead broken them down into issues for each component

### Subject issues
1. No subject
2. Emails

### Body issues
1. Hyperlinks & Websites
2. Subject lines
3. Image references
4. Page/Line breaks
5. Timings
6. Documents (.xls, .doc, .pdf)
7. Emails less than 4 words, and from 1000 words
    - Discard as these are not meaningful
8. Infrequent senders (less than 3 messages)
    - Discard as these people would not add meaningful results
9. Email signatures
    - Discard as these are independent of topics.

![signature]({{site.baseurl}}/assets/img/signature.jpg)

### User issues
1. Users may have multiple email addresses
    While this is not a data issue per se, it is an issue to be addressed later.

## Final Results
Before we get into the results of what it should look like, I would like to add that the act of reviewing data is sometimes rewarding not just from understanding the data given but also, sometimes.. you do find really interesting emails. [clean jokes](http://www.enron-mail.com/email/rogers-b/deleted_items/Fwd_FW_zen_for_the_day.html), [dirty jokes](http://www.enron-mail.com/email/gay-r/sent/Etiquette_rerun_17.html), and even acts of possible [infidelity](http://www.enron-mail.com/email/gay-r/sent/RE_Happy_Valentine_s_Day_1.html)

But lets not get distracted. For purposes of performing our topic modelling, we would drop duplicate emails
We would save the result after cleansing, and this is how it should look:
![Final result]({{site.baseurl}}/assets/img/final_result.jpg)
