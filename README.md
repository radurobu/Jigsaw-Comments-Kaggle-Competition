# Jigsaw-Comments-Kaggle-Competition
Jigsaw Rate Severity of Toxic Comments SILVER Medal

**This repository presents my approach for the "Jigsaw Rate Severity of Toxic Comments" Kaggle competition. My approach ranked 91 out of 2.301 teams and rewarded a Kaggle silver medal.**

## Description:

In this competition, we had to score a set of about fourteen thousand comments. Pairs of comments were presented to expert raters, who marked one of two comments more harmful — each according to their own notion of toxicity. In this contest the scores will be compared with several hundred thousand rankings. The average agreement with the raters determines the individual score. 

## Approach:

In this competition the goal was to make use of Natural Language Processing models in order to rank comment toxicity.
My approach consisted of an ensemble of two models, one Ridge Regression that used TFIDF for text encoding and the second was a Random Forest model that used word-embeddings from a pre-trained ROBERTA model.

First step was some simple text cleaning:
    1. Remove special characters like &, #, etc
    2. Removes extra spaces
    3. Removes embedded URL links
    4. Removes HTML tags
    5. Removes emojis
    
I observed that toxic comments where mostly written in upper case. One important feature in my model was the percentage of upper characters in the comment (comments with higher upper-case letters where generally more toxic)

Quite surprising that linear models (such as Ridge Regression) performed verry well in this competition. I used the classic "term frequency–inverse document frequency" TFIDF approach to encode the word tokens. The ridge regression received 80% weight in the final ensemble. Second, I used the pre-trained Roberta-Base model from Hugginface to transform the text data into word-embeddings (vector encoding). The vectors where further used as explanatory variables in an Random Forest Classifier with 100 estimators and max_depth=32.

Ensembling the output of a linear ridge regression model and the output of an RF Classifier obviously will not work verry well, since the classifier is a probability (bounded between 0 and 1) and the output of the linear regression is basically unbounded. One trick I did that seemed to work was to transform the output of the linear model to resemble a probability, by applying the logistic function as shown below.

    from sklearn.preprocessing import MinMaxScaler
    scaler = MinMaxScaler(feature_range=(-6,+6))
    pred_scale = scaler.fit_transform(pred.reshape(-1, 1))
    avg_pred_scale = np.mean(pred_scale)
    pred_scale = pred_scale - avg_pred_scale
    pred_scale = 1/ (1 + np.exp(-pred_scale))

I didn't trust public LB and worked towards improving the score on the un-processed validation data.

## Conclusion:
Sometimes we don't need to use fancy state of the art NLP models (such as transformers). In some cases, just the classic TFIDF and a linear model can work equally well.
