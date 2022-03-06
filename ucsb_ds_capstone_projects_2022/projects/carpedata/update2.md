# Update 2 - (3/4/22)
******

## RECAP

In our last progress report, we:
* Provided a summary of our dataset and stated the 6 unique labels: 
  * N/A: No Relevant Content, Possible Fatality, Potentially Unlawful Activity, Physical Activity, Information related to the claim, Potentially relevant information
* Described our data cleaning process
  * We have a cleaning pipeline. We used BeautifulSoup to remove links, make all of the text lowercase, remove non-alphanumeric symbols, etc. We used spaCy to tokenize the text.
* Explained our train-test split, where our testing set is 20% of the cleaned data
* Summarized our three models: Logistic Regression, Naive Bayes, and SVM
  * For each model, we used a stemmed dataset and lemmatized dataset with the intent to determine if stemming or lemmatization is better.
  * For each model, we provided the precision, recall, and total accuracy scores.
  * We determined that SVM was the best model so far because it had the highest precision, recall, and accuracy score (each one was 85%). However, this model is still not great due to the multiple commission errors (false positives) shown through our confusion matrix.
* Proposed future work
  * We dropped the three smallest classes which had a total of only six observations. We explained that we would try to recombine classes in a different manner. 
  * We stated that we might start tuning hyperparameters for SVM, since it is our best model so far.
  * We said that we might try out even more models such as Decision Tree/Random Forest, Light Gradient Boosting Machine, and Neural Network.



## PROGRESS

### PREPROCESSING

With regards to preprocessing, we:
* Attempted to drop the six lowest class labels that all had less than 100 observations from our training and validation. 
  * This would allow us to train our models without these small classes and then use the models to see what these small classes would be predicted as. After dropping the smallest classes, we trained both a logistic regression and a support vector machine with the default hyperparameters and used both models to predict over the text for the dropped classes. As a result, both models predicted most of the smaller classes to be N/A: No relevant content, which makes sense as that specific class has the most observations by far. 
  * We presented these findings to our sponsor who told us to just drop the six smallest classes from the dataset and to state in our final report that these classes exist in the real world, but are not worth keeping in our models as their frequencies are so low. 
* Decided to solely use lemmatization as opposed to creating models for both the lemmatized and stemmed forms of the text.
* Decided to use a TFIDF-vectorizer as opposed to both a TF-IDF and a CountVectorizer just for simplicity sake.
* Decided to add in bigrams within our preprocessing pipeline
  * Instead of using solely using unigrams when vectorizing the text column, we decided to create models with only unigrams as well as models with both unigrams and bigrams.
  * According to our sponsors and mentor, in most NLP related models, the addition of bigrams creates a significant improvement in terms of accuracy.
  * This also created a challenge for us as the number of columns when using both unigrams and bigrams increases from about 400,000 to about 5 million. One solution that our sponsors provided us with is to use different forms of dimensionality reduction.
* Attempted to reduce the dimensions of the vectorized dataframe through different methods.
  * NMF: Non-Negative Matrix Factorization: Finds two non-negative matrices (W, H) whose product approximates the non-negative matrix X.
    * Worked fine for logistic regression, naive bayes, and the support vector machine.
  * Truncated SVD: This transformer performs linear dimensionality reduction by means of truncated singular value decomposition (SVD).
    * Produces negative values which does not work for the naive bayes model but worked fine for the logistic regression and the SVM.
  * We compared the use of each dimensionality reduction to the models where we do not use dimensionality reduction and while it reduces computation time, the reduction does not produce better results in terms of precision, recall or F1 score.
* Removed uncommon words that appear in less than 5% of the documents.
  * This allows us to speed up computation time by removing words that will have little effect on classifying the document into one of the types of fraud, solely due to the infrequency of the word.

### MODELING

Since our last progress report, we:
* Decided upon a modeling metric to look at in order to determine which models we prefer.
  * The modeling metric we decided to use is Weighted Average Precision. We chose a weighted average over a macro average due to the imbalance in size between classes.
  * Comparing this metric for all the models allows us to determine which models are “better” when doing cross validation to choose hyperparameters. 
* Opted to not use the balanced class weights parameter in our models.
  * In our last progress update, we mentioned the possible use of the balanced class weights parameter which would allow the model to better capture smaller classes. However we showed our results to the sponsors who said that the decrease in total accuracy is not worth the increase in accuracy for the smaller classes.
* Still have not moved on to other models as we are still working on cross-validating in order to tune hyperparameters for our existing models

### VALIDATION

Since our last progress report, we implemented a grid search cross-validation for all of our models.
* For naive bayes, we consider the following hyperparameters:
  * A smoothing parameter for additive smoothing or no smoothing
  * A parameter whether to learn class prior probabilities or to assume a uniform prior
* For linear regression, we consider the following hyperparameters:
  * Regularization parameter
  * ElasticNet ratio for Lasso regression, Ridge regression or a combination of both penalties
* For our SVM classifier, we consider the following hyperparameters:
  * Regularization parameter
  * Kernel type


### VISUALIZATION

In terms of visualization, we:
* Created word clouds for the top token features for the naive bayes model using lemmatization, a TFIDF vectorizer, and unigrams.
  * There is one word cloud for each of the top features for each class label. For example, we have one word cloud for the “possible fatality” class, one for the “physical activity” class, etc.
  * Having one for each class allows us to see which tokens had the most importance in terms of classifying the documents as that specific class.
  * Below displays the word clouds for the top 55 words for each class.

<p align="center">
    <img src="https://i.imgur.com/njWjZcS.png" style="height: auto;"></img> <br>
</p> 



## CONCLUSION

All in all, in the last couple of weeks, we:
* Improved our data cleaning and preprocessing by
  * excluding the data of the six lowest class labels in our whole dataset
  * focusing solely on lemmatization instead of stemming due to higher accuracy
  * adding bigrams	for higher accuracy
  * testing dimension reductionality methods NMF and Truncated SVD for performance and accuracy
  * filtering out low frequency words that were not improving model accuracy for the sake of performance
* Tweaked our process for modeling by
  * establishing our standard metric, Weighted Average Precision
  * removing the balanced weights class parameter in our model for the sake of accuracy
* Began a grid search cross-validation for our naive bayes, linear regression and SVM models in order to test out hyperparameters
* Created exploratory word clouds for the top token features of our labels
Ultimately, we improved the cleaning and modeling pipeline that we had previously established while also incorporating new exploratory plots and the start of cross-validation. 


## FUTURE WORK

Going forward, we plan to:
* Finish tuning our SVM model and begin experimenting with LightGBM models
* Look into training a neural network
* Use dimensionality reduction 
  * Computing time has increased drastically as we have began to cross-validate and delve into more complex modeling techniques
  * However, NMF has not produced sufficient results, so we will attempt to use Truncated SVD going forward and measure the relevant performance
  * Additionally, we plan to use the “min-df” and “max-df” hyperparameters in our TF-IDF Vectorizer to reduce the amount of tokens and cross-validate for the optimal values
* Utilize our collated word cloud to:
  * look into top tokens for different labels
  * edit stop word list for redundant tokens to increase categorizing accuracy based on the visual cues from our word clouds
* Work on finalizing model selection by:
  * creating visualizations to compare model average precision and overall performance
  * utilizing ROC curves and AUC more in model selection
  * finding important features in our dataset through their Shapley values

