# Stack_overflow_tags_prediction

![alt text](https://github.com/Muk200/Stack_overflow_tags_prediction/blob/main/pic1.jpg)

Stack Overflow is the largest, most trusted online community for developers to learn, share their programming knowledge, and build their careers.

Stack Overflow is something which every programmer use one way or another. Each month, over 50 million developers come to Stack Overflow to learn, share their knowledge, and build their careers. It features questions and answers on a wide range of topics in computer programming. The website serves as a platform for users to ask and answer questions, and, through membership and active participation, to vote questions and answers up or down and edit questions and answers in a fashion similar to a wiki or Digg. As of April 2014 Stack Overflow has over 4,000,000 registered users, and it exceeded 10,000,000 questions in late August 2015. Based on the type of tags assigned to questions, the top eight most discussed topics on the site are: Java, JavaScript, C#, PHP, Android, jQuery, Python and HTML.

# Problem Statemtent

Suggest the tags based on the content that was there in the question posted on Stackoverflow

# Objectives and constraints
1. Predict tags with high result & precision
2. incorrect tags do impact
3. no strict latency

# Problem formation
D = {xi, yi} as sets, aka "multiclass" classification

# Data Overview

Refer: https://www.kaggle.com/c/facebook-recruiting-iii-keyword-extraction/data All of the data is in 2 files: Train and Test.
Train.csv contains 4 columns: Id,Title,Body,Tags.
Test.csv contains the same columns but without the Tags, which you are to predict.
Size of Train.csv - 6.75GB
Size of Test.csv - 2GB*
Number of rows in Train.csv = 6034195

# Data Field Explaination

Dataset contains 6,034,195 rows. The columns in the table are:
1. Id - Unique identifier for each question
2. Title - The question's title
3. Body - The body of the question
4. Tags - The tags associated with the question in a space-seperated format (all lowercase, should not contain tabs '\t' or ampersands '&')

# Performance metric
**Micro-Averaged F1-Score (Mean F Score) :** The F1 score can be interpreted as a weighted average of the precision and recall, where an F1 score reaches its best value at 1 and worst score at 0. The relative contribution of precision and recall to the F1 score are equal. The formula for the F1 score is:
F1 = 2 * (precision * recall) / (precision + recall)

**In the multi-class and multi-label case, this is the weighted average of the F1 score of each class.**

**'Micro f1 score':** Calculate metrics globally by counting the total true positives, false negatives and false positives. This is a better metric when we have class imbalance. Takes frequency into consideration as weighted

**'Macro f1 score':** Calculate metrics for each label, and find their unweighted mean. This does not take label imbalance into account.

https://www.kaggle.com/wiki/MeanFScore http://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html

**Hamming loss :** The Hamming loss is the fraction of labels that are incorrectly predicted. https://www.kaggle.com/wiki/HammingLoss


# Preprocessing

1. Sample 1M data points 
2. Separate out code-snippets from Body
3. Remove Spcial characters from Question title and description (not in code)
4. Remove stop words (Except 'C')
5. Remove HTML Tags
6. Convert all the characters into small letters
7. Use SnowballStemmer to stem the words

![alt text](https://github.com/Muk200/Stack_overflow_tags_prediction/blob/main/img.png)


# EDA tag anaalysis
1. Number of times tag appeared
2. sort them in descending order
3. plot quantites for top 100
4. has a huge skewness

# Converting tags for multilabel problems

|   X  |  y1  |  y2  |  y3  |  y4  |
|------|------|------|------|------|
|  x1  |   0  |   1  |   1  |   0  |
|  x1  |   1  |   0  |   0  |   0  |
|  x1  |   0  |   1  |   0  |   0  |


# Featurizing data
only text is left for tfidf, has one gram & h grams, it is a **sparse matrix** 

since we have high dimension data and many modules we use logistic regression with one vs rest classifer. 
Training log reg is cheap.  Use 0,5 million - 500 labels - for more efficient weight tot title(3x).

Building  model with - SGDC classifier with log loss
                     - logistic regression with L1 Regularization
                     - or [lrSVM + SGDC classifier + hinge loss ]

**why only simple Linear regression?** 
because of high dimensionality data and time complexity 
