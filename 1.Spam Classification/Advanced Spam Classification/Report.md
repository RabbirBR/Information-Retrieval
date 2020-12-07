# Report

**Team ID:** 004

**Student Name:** Rabbir Bin Rabbani


## Standard features and algorithms
Despite the extra work, the accuracies, precision and the False Positive Rate for the TF and TF-IDF seems to be worse than the simple term count vector. The best results seems to be for the SVM with the term count, which I have put in bold in the table below.

#### Results
The train-validation split was done using sklearns "*train_test_split()*" function.


| Algorithm | Term weighting | Acc. | Prec. | FPR |
| -- | -- | -- | -- | -- |
| Naive Bayes | Count | *0.925* | *0.895* | *0.155* |
| Naive Bayes | TF | *0.895* | *0.847* | *0.241* |
| Naive Bayes | TF-IDF | *0.910* | *0.867* | *0.206* |
| **SVM** | **Count** | ***0.982*** | ***0.981*** | ***0.026*** |
| SVM | TF | *0.961* | *0.952* | *0.067* |
| SVM | TF-IDF | *0.967* | *0.956* | *0.061* |

## Experimental approaches
I chose the following features:
* **Subject Length -** From observation, the subject length of spam emails tend to be bigger than non-spam emails since people prefer to use short and precise subjects for everyday emails.
* **Body Length -** On the other hand, the body length of spam emails seem to be in a certain range, but non-spam emails can be shorter or longer.
* **Return-Path -** On closer Inspection, non-spam emails often have a return path property which I'm also using.

I chose to use 4 different machine learning algorithms including the Naive-Bayes and SVM we used in the first task since they give quite good accuracy. The other two are as follows:
* **Logistic Regression -** Logistic Regression is good for Binary Classification problems in general. I have used Logistic Regression to classify text before, during last semester in data mining course and it gave me very good accuracy, so I decided to try it here as well.
* **Decision Tree -** Since we are "deciding" if an email is spam or ham. It seemed fitting to try out the Decision Tree, which is not a bad idea since Decision tree gives a very good accuracy.

Additionally, I did not use TF or TF-IDF for my experimental approaches since they gave worse result during step 1.

#### Text pre-processing
For each email, First I extracted the email subject and body from email using the python *email* library, then removed all numbers and special characters using regular expressions. I used "*nltk*" library to remove all stopwords and also all words that are not part of the english dictionary and then I remove one letter words such as *'b'*, *'e'* since they will hold no relevance in the classification.

Finally I create a csv file of the data since this process takes about 30 minutes in my computer to process and to save time I have kept the processed train and test data in separate csv files. The data files can be downloaded from [this dropbox link](https://www.dropbox.com/sh/htywy8h8rtbm3u7/AAD_zpLznXeBnMMQIzPJa8lda?dl=0).


#### Results
Even though I have used two new Machine Learning Algorithms which gives very good accuracies on their own, SVM still gives the best accuracy among the four Machine Learning Algorithms.

| Algorithm | Features | Other choices | Acc. | Prec. | FPR |
| -- | -- | -- | -- | -- | -- |
| Naive Bayes | Term Count | All 3 Additional Features | *0.878* | *0.831* | *0.270* |
| **SVM** | **Term Count** | **All 3 Additional Features** | ***0.991*** | ***0.991*** | ***0.011*** |
| Logistic Regression | Term Count | All 3 Additional Features | *0.975* | *0.987* | *0.017* |
| Decision Tree | Term Count | All 3 Additional Features | *0.989* | *0.991* | *0.012* |

## Discussion
* I also tried counting the how many recievers a certain mail has, but it did not affect the accuracy for me while also using the 3 features I chose, so I ended up not using it.
* During preprocessing I tried to use the stemmer in nltk, but it did not significantly improve my results so I removed it for later versions.

For my submission in Kaggle, I used SVM with term count with the three additional features. The three features approximately improved my accuracy by 1%, but they significantly decreased the False Positive Rate.

<!--
## References
  * *If you used external resources (websites, books, articles, etc.) make sure you acknowledge them here.*
-->