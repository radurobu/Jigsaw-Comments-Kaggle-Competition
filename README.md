# Jigsaw-Comments-Kaggle-Competition
Jigsaw Rate Severity of Toxic Comments SILVER Medal

*This repository presents my approach for the "Jigsaw Rate Severity of Toxic Comments" Kaggle competition. My approach ranked 91 out of 2.301 teams and rewarded a Kaggle silver medal.

## Description:

In this competition, we had to score a set of about fourteen thousand comments. Pairs of comments where presented to expert raters, who marked one of two comments more harmful — each according to their own notion of toxicity. In this contest the scores will be compared with several hundred thousand rankings. The average agreement with the raters determines the individual score. 

## Approach:

In this competition the goal was to make use of Natural Language Processing models in order to rank comment toxicity.
My approach consisted of an ensemble of two models, one Ridge Regression that used TFIDF for text encoding and the second was a Random Forest model that used word-embeddings from a pre-trained ROBERTA model.

First step was some simple text cleaning:
    1.Remove special charecters like &, #, etc
    2. Removes extra spaces
    3. Removes embedded URL links
    4. Removes HTML tags
    5. Removes emojis
    
I observed that toxic comments where mostley written in upper case. One important feature in my model was the perchantage of upper characthers in the comment (comments with higher upper-case letters where generaly more toxic)

Quiet surprising that liniar models (such as Ridge Regression) performed verry well in this competition. I used the clasic "term frequency–inverse document frequency" TFIDF approach to encode the word tokens. The ridge regression received 80% weight in the final ensemble.

Second, I used the pre-traine Roberta-Base model from Hugginface to encode..

I didn't trust public LB, and worked towards improving the score on the un-processed validation data.
## Conclusion:
