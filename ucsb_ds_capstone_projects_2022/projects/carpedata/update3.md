# Update 3 - (4/29/22)
******

In our last progress report, we refined our cleaning and preprocessing pipeline in order to ensure that the models we created would perform more efficiently and accurately in predicting the labels for our web page data. We then finished creating our baseline models for naive bayes, linear regression and SVM and began cross-validation across all of them in order to tune the hyperparameters. We also added various exploratory plots in order to visualize model performance and the features of the labels in our dataset.

During this process, we encountered a few issues regarding the large dimensionality of our features as well as computation time. We also wanted to explore whether more complex algorithms such as LightGBM could improve the accuracy of our models. We discuss our approaches and our findings for these goals in the following progress report.

## **FINALIZING OUR PREVIOUS MODELS**

The models that we needed to finish hyperparameter tuning were the logistic regression, naive bayes, and support vector machine models. We first began by doing some more exploratory analysis to see the document frequency for words within our dataframe. This would allow us to determine how to use the min_df parameter within our vectorization function.

Through our analysis, we discovered that over 64% of our unique tokens only occur in 1 document. From there we decided to remove all tokens that only appear in one document, as they do not add much to our model while slowing down computation time as well. Below is a bar graph showing counts of document frequency.

<p align="center">
    <img src="https://cdn.discordapp.com/attachments/949346720536481802/969721724587348058/image2_bar.png" style="width: 500px; height: auto;"></img> <br>
    <em>Figure 1. Count of documents and their word counts in dataset.</em>
</p>

The methodology for creating this graph involves using the “corpora” function from the “gensim” library to create a dictionary of tokens and their corresponding document frequencies. From there we just had to convert the values of the dictionary to a numpy array and then plot a histogram for all the tokens that appear in less than 10 documents.

The next thing we did was add the token “com” to our list of stopwords, thus removing it from our dataframe. We noticed in our word clouds that “com” was appearing rather frequently due to us not removing it during our cleaning pipeline.

We also added in a micro average ROC curve for the majority of our models. Below is the ROC curve for our logistic regression model using bigrams.

<p align="center">
    <img src="https://cdn.discordapp.com/attachments/949346720536481802/969721724344102962/image1_roc.png" style="width: 600px; height: auto;"></img> <br>
    <em>Figure 2. ROC Curve curve for our logistic regression model using bigrams.</em>
</p> 

The last thing we did was finish up tuning the hyperparameters for our SVM model. The parameters we looked at are the probability parameter as well as the C parameter. The best model produced a testing weighted average precision of **85%** which so far has been our best performing model. This model was only run on unigrams. We were not able to run it on bigrams due to computation issues. From there we were able to move on to different boosting models.


## **IMPLEMENTING A DECISION TREE & RANDOM FOREST CLASSIFIER**

After researching advanced classification models and consulting our sponsor, we knew we wanted to implement a Light GBM model due to its speed and efficiency. Thus, we decided to implement a Decision Tree and Random Forest Classifier as baseline models to compare to the LightGBM due to the fact that LightGBM is a gradient-boosted decision tree. 

For our Decision Tree classifier, the main parameter that tuned was max_depth which is the maximum depth of a tree. In our cross-validation, we found that max_depth = 15 was the best for our datasets. Below is a table with the results of our model with our different data — unigrams vs. bigrams and the removal of words that occur in only one document.

<p align="left">
    <b>Decision Tree Model Comparison</b>
</p>

<center>
  
| Data      | Weighted Average Precision |
| ----------- | ----------- |
| Bigrams      | 0.80       |
| Bigrams, min1doc   | 0.79        |
| Unigrams      | 0.80       |
| Unigrams, min1doc   | 0.81        |

</center>

<p align="left">
    <em>Figure 3. A comparison of the weighted average precision metrics according to the different data used in the decision tree model.</em>
</p>

We see that the Decision Tree model has an weighted average precision of about 79-81%, which is a bit lower than our previous models. Interestingly, the model containing data from bigrams did not perform much better than the model with only data from unigrams.

For our Random Forest classifier, we tuned the parameters max_depth and n_estimators. Again, below is a table with the results of our model with our different data — unigrams vs. bigrams and the removal of words that occur in only one document.

<p align="left">
    <b>Random Forest Model Comparison</b>
</p>

<center>
  
| Data      | Weighted Average Precision |
| ----------- | ----------- |
| Bigrams      | 0.82       |
| Bigrams, min1doc   | 0.83        |
| Unigrams      | 0.82       |
| Unigrams, min1doc   | 0.82      |

</center>

<p align="left">
    <em>Figure 4. A comparison of the weighted average precision metrics according to the different data used in the random forest model.</em>
</p>

At first glance, we see that the Random Forest classifiers did perform slightly better than the Decision Tree classifiers with weighted average precisions of 82-83%, but not by much. This value is close to our current best SVM model, but does not look to be better.

We also see again that the data with bigrams did not increase the model performance much compared to the data with unigrams. This was odd to us, as we thought bigrams would be able to provide the model with more relevant information, and the models using data with bigrams definitely took longer to process.

The good thing about decision trees is that they are relatively easy to interpret and understand. However, given the fact that we have a large dataset, interpretability is not a major concern. Even so, the Decision Trees and Random Forest classifiers were relatively easy and straightforward to tune and implement, especially compared to more advanced and complex models. Ultimately, both models performed relatively well as baseline models to compare for LightGBM.

## **IMPLEMENTING A LIGHTGBM CLASSIFIER**

After completing the decision tree and random forest models as a baseline, the main model that we wanted to focus on this quarter was a LightGBM. This model was suggested to us by our sponsors as it is one of the models that they use the most for NLP related models.

* PROS: After doing our own research on LightGBM we came to the conclusion that out of all the boosting models, it was one of the fastest in terms of computation time as well as producing one of the highest accuracies, only behind a CatBoost model [1].
* CONS: We determined that while this model is newer and more complex than the models we have previously implemented, it is often prone to overfitting which is why our sponsors told us to tune certain hyperparameters.
* The hyperparameters we chose to tune using a GridSearchCV were:
    * Learning rate (helps with accuracy)
    * Max_depth
    * Num_leaves (both max_deph and num_leaves work in conjunction to help prevent overfitting) [2]

After tuning hyperparameters and running the model on the test set, we obtained a weighted average precision of **84%** for bigrams which to our surprise was less than that of the SVM. Classification report shown below.

<p align="center">
    <img src="https://cdn.discordapp.com/attachments/949346720536481802/969721724818063380/image3_report.png" style="width: 500px; height: auto;"></img> <br>
    <em>Figure 5. Classification report of LightGBM model.</em>
</p> 

After seeing that our LightGBM performed worse than our SVM we were a little thrown off as to how to proceed so our sponsors emphasized the importance of dimensionality reduction and feature selection as our dataset is so wide (over 300,000 columns). Because of this, our sponsors suggested that we increase our min_df parameter to only include tokens that appear in 10+ documents as well as to use a pre-trained language model such as word2vec to decrease the number of features that our model is taking as input.

## **EXPLORING PRETRAINED LANGUAGE MODELS**

We plan to implement pretrained language models such as Word2Vec and BERT in the near future. Word2Vec is a natural language processing technique that utilizes a neural network to create word embeddings. The input is a large corpus of text and Word2Vec learns to output the corresponding vector representation [3].

 Doc2Vec is another natural language processing technique that is a generalization of Word2Vec. Doc2Vec is “used to create a vectorized representation of a group of words taken collectively as a single unit” [4]. The difference between Word2Vec and Doc2Vec is that Word2Vec is trained on individual words while Doc2Vec is trained on large bodies of text.

BERT stands for Bidirectional Encoder Representations from Transformers. BERT uses the Transformer encoder which reads a sequence of words together; this is useful because the model will be able to learn the context of the word based on the words that surround it [5]. We want to implement the Word2Vec and BERT models in order to determine whether or not these models will produce a high accuracy score and perform better than the models we have already implemented.

## **CONCLUSION & FUTURE WORK**

In our previous progress update, most of our work had been focused on fine-tuning the basic classification models that we were using, with an emphasis on the specific hyperparameters we were tuning. Since then, we have forayed significantly into more statistically flexible models, like the LightGBM classifier and the Random Forest classifier, in order to make sure that we cover all possible classification models in our search for the top performer. Through this process, we realized that these new classification methods were not performing better than our previously trained SVM model. Additionally, we experimented with the ‘min_df’ and ‘chi squared’ hyperparameters in our TF-IDF vectorizer and haven’t seen much improvement in terms of results either.

While our efforts haven’t resulted in the exact success that we want, they have given us perspective of what we want to do going forward:

* We will continue to look into pretrained language models, such as Word2Vec, to see if an out of the box solution is more viable.
* We will continue to research and possibly alter the hyperparameters we are tuning.
* We plan to refine our data cleaning pipeline even further with an emphasis on eliminating unnecessary words.
* If time permits, we may possibly train a neural network in hopes of achieving higher accuracy scores.


## **REFERENCES**

1. [https://towardsdatascience.com/boosting-showdown-scikit-learn-vs-xgboost-vs-lightgbm-vs-catboost-in-sentiment-classification-f7c7f46fd956](https://towardsdatascience.com/boosting-showdown-scikit-learn-vs-xgboost-vs-lightgbm-vs-catboost-in-sentiment-classification-f7c7f46fd956)
2. [https://towardsdatascience.com/kagglers-guide-to-lightgbm-hyperparameter-tuning-with-optuna-in-2021-ed048d9838b5](https://towardsdatascience.com/kagglers-guide-to-lightgbm-hyperparameter-tuning-with-optuna-in-2021-ed048d9838b5)
3. [https://www.section.io/engineering-education/what-is-word2vec/#what-is-word2vec](https://www.section.io/engineering-education/what-is-word2vec/#what-is-word2vec)
4. [https://www.tutorialspoint.com/gensim/gensim_doc2vec_model.htm](https://www.tutorialspoint.com/gensim/gensim_doc2vec_model.htm)
5. https://towardsdatascience.com/bert-explained-state-of-the-art-language-model-for-nlp-f8b21a9b6270
