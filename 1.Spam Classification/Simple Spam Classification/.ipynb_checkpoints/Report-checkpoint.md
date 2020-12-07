# Report

**Team ID:** 004

**Name:** Rabbir Bin Rabbani

## Approach
We know, that some words are used more in spam emails than non-spam emails (Ex: Free, Guaranteed, Win, etc). So, if the classifier can learn how to find the most frequent words in spam and ham emails from a training set then it should be able to predict if a given email is spam with reasonable accuracy.

But the words should come from only the subject and the main body of the email. So, before the classifier can be taught anything, the mail subject and body is extracted and put into a dataframe using the "*process_email_data()*" function. Afterwards, the training dataset is split into train and validation sets.

During training, the classifier takes all spam and ham emails from the training set, and makes a list of the most frequent spam and ham words, then saves them for prediction.

During prediction, the classifier calculates how many words in the given email matches with the frequent spam and ham words found during training. If there is more matches with the spam words, then the email is labeled as spam, otherwise it is labeled as ham.

During initialization the classifier object takes a variable called "*frequent_word_percentage*", this is used to control what percent of the most common words to use during training.

The model is then trained using the entire dataset and then it is used to predict for the test dataset.

## Experimental setup
After the preprocessing, the dataset is available as a dataframe, so I have used the "*train_test_split()*" function from Sklearn to separate the data into training and validataion sets.

## Results
This model is able to detect a spam E-mail 82.5% of the time.

Of the times that the model does say that the email is spam it is right 82.8% of the time. 

In the case that the email is actually not a spam email, the model will wrongly say it is spam only 4.6% of the time.

Even though I have given the macro- and micro-averaged results, I am choosing The micro-averaged results, because the training data provided was very balanced in an almost 50-50 split.

| Model | Accuracy | Macro Precision | Micro Precision | FPR (Macro) | FPR (Micro) |
| -- | -- | -- | -- | -- | -- |
| Simple Email Classifier | *0.828* | *0.841* | *0.828* | *0.264* | *0.046* |

## Discussion (max. 200 words)
As mentioned before, some words are used more in spam emails than non-spam emails. Both of the approaches I tried was based on this knowledge.

My first idea was to make a histogram of the most frequent spam and ham words. and if the new email had a more similar histogarm to the spam histogram than the ham histogram, then it would be labeled as spam. But this idea was flawed. During validation, the accurracy score was only 0.69 and it was further proven by my first submission in the kaggle competition which got an accuracy score of only 0.61. 

In my second idea, I opted to simply check how many of the spam and ham words were present in the email to be labeled, this gave me an accuracy of 0.828 during validation and a score of 0.829 on the kaggle competition submission.

## References

<!-- *If you used external resources (websites, books, articles, etc.) make sure you acknowledge them here.* -->
