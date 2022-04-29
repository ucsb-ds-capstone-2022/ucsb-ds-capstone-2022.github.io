# Using Images for Recommendation

Images have been shown to have vast predictive power for various tasks. When broken down, images are a 2D array of pixels, with each pixel consisting of 3 color channels (Red, Green, and Blue). Within that set of pixels lie plenty of datapoints. Let’s say an image in our data set has dimensions of 384 x 512 x 3. That yields nearly 600,000 data points in just one image. Clearly each pixel’s value does not hold the same predictive standing as an individual feature of the model, but the ensemble of pixels can open up patterns in a user’s image preferences that are difficult for humans to detect. For example, a user may have a predilection towards properties with green lawns out in front. Or perhaps user’s are drawn to photos with more vibrant color contrasts. Either way, a user’s interaction with images can be valuable in determining their preferences, and since we are lucky enough to have images associated with a majority of our properties, we hope to harness the influence that they have to offer.

## Image Preprocessing

Working with images requires various techniques of preprocessing before being used as input to a model. We had to spend some time researching how we can utilize the information stored in images and put it into our model. 

We first needed to map our houses with their corresponding images and then vectorize them. But because the images are different sizes and different data types, we had to figure out a way to make all the images the same sizes so it will work as a modality feature. What we decided to do was to flatten the image tensors so they were all one dimension and to pad the arrays so they are all the same size. 

However doing this means we are forcefully making the tensors the largest possible size it could be, which in some cases could consist of 150,000 elements. This is a problem because it is computationally expensive to go through all 150,000 elements for all of the images. One way to fix this is to force each vector to be a certain size; however, by doing this, we lose some predictive power and parts of the images that might be important. We explored other ways of preprocessing our images to make them workable in our models, such as interpolation, dimension reduction techniques, and transfer learning. 



## Using Transfer Learning For Image Feature Extraction

Each image contains a large number of pixels, which when dealing with thousands of images can affect our training time or even make the model unusable. To address this issue, we investigated the idea of using image pre-processing tools to reduce the dimensions of our images to a reasonable number. A powerful tool that has been shown to extract features from images is using a large pre-trained convolutional neural net. Though there are a few different available pre-models, we ended up choosing VGG16 since it is a tried and tested option. It is not the most recently updated pretrained CNN, but it still provides excellent results. Its most evident drawback, however, is its large number of parameters which makes it a bit costly. 



![VGG16 Architecture
](https://cdn.discordapp.com/attachments/927717200247275561/969647620916138015/unknown.png)

We were able to use VGG16 to convert our images to a 4096 1D-array. After attaining the pre-processed values for each image in the dataset, we appended the respective arrays to our item features in order to run experiments with the image data. Part of our future goals include investigating other image pre-processing models. A newer set of models, like the EfficientNet model, has been shown results of a similar quality to those of previous models but with a fraction of the parameters. 

## Image Implementation

To visualize the effect of our image data, we used a visual (includes images) and non visual (does not include images) bayesian personalized ranking models with our VGG16 preprocessed data. Bayesian Personalized Ranking (BPR) is a collaborative filtering method that uses a pairwise ranking optimization framework which adopts stochastic gradient ascent as the training procedure. 

![Visual Bayesian Personalized Ranking Architecture ](https://cdn.discordapp.com/attachments/927717200247275561/969647678919155722/unknown.png)

The visual version of BPR (VBPR) uses a method that incorporates visual features extracted from a pre-trained CNN and learns an embedding kernel which linearly transforms such high-dimensional features into a much lower-dimensional (say 20 or so) ‘visual
rating’ space. This is then used in the normal matrix factorization based predictor function of BPR. The only difference between VBPR and BPR is the incorporation of images and therefore is a great way to compare the inclusion of image modality.


![Experiment results of Visual and Non-visual Bayesian Personalized Ranking ](https://cdn.discordapp.com/attachments/927717200247275561/969647755435847680/unknown.png)


As you can see above, the VBPR model that includes images performs better than BPR on every metric. This provides proof that implementing images may be beneficial for our model. 

Now that we have some results of images benefitting the model, we plan on setting up solid baselines with them by hyperparameter tuning. We have 3 we would like to hyperparameter tune: VBPR, BPR and another image focused recommender model called Causal Rec. We’re gonna spend some time reading up on these methods and figure out which hyperparameters to use and tune.

# Feature selection

With images now added to our feature set, we have plenty of item features in our dataset to use. Including as many features as possible does not always improve recommendation accuracy, and more importantly can reduce our efficiency due to large training times. Therefore, we wanted to find which ones are the most important. Feature selection allows us to choose the best variables for our model and eliminates any redundant and unimportant variables. This increases the predictive power and efficiency of our model. 

The method we chose to perform feature selection is a forward selection. Forward selection is an iterative, greedy algorithm that starts with no features and adds the best performing feature until all combination sizes are exhausted. For example, if there are 16 features we would go through all 16 features and select the best performing one. Then, we would test the rest of the features with our best performing feature and add on the feature that performed best. We would iterate through this process until we get the combination of all 16 features.

There are many features that we have to go through. The biggest challenge is that we haven’t found an automated method to perform forward feature selection for our model, so we have to manually test each feature and combination of features. This has been very time consuming, and computationally expensive. In addition, due to the timeliness of this process, we have had to compromise some of our accuracy in testing for us to get through the entire process of feature selection. We have had to reduce our dataset in half and train on only 5 epochs.

So far, we have been able to test all our individual features and found that ‘latitude’ and ‘longitude’ (which we treated the combination as one feature) produced the best score in the metric we are focusing on (Mean Average Precision). Next we hope to continue the forward selection process and find the best combination of features.


# Hybrid Models

The most powerful modern recommender systems are a set of hybrid models using a combination of collaborative filtering and content based methods. Our current model, the bilateral variational autoencoder, is a hybrid model but leans more towards a collaborative filtering approach since item/user features are only used as priors. Due to this limitation, we believe greater performance can be achieved using a complete hybrid approach and fully using the power of both sparse and dense features. We decided to explore various hybrid models in Microsoft’s recommender package. 

We first started with LightFM. The LightFM model represents users and items as linear combinations of their content features’ latent factors. The model learns embeddings or latent representations of the users and items in such a way that it encodes user preferences over items. These representations produce scores for every item for a given user; items scored highly are more likely to be interesting to the user. 

The second model we tried was Wide and Deep.  Wide and Deep is a linear model with a wide set of crossed-column (co-occurrence) features that can memorize the feature interactions, while deep neural networks (DNN) can generalize the feature patterns through low-dimensional dense embeddings learned for the sparse features. Wide-and-deep learning jointly trains wide linear models and deep neural networks to combine the benefits of memorization and generalization for recommender systems.

We have run these models on a small subset of our data and they have shown promise. We hope to run them on our full dataset soon and provide strong baselines for hybrid models.

We also explored pytorch’s newly released Torchrec library but found it lacking documentation and examples leading to endless errors. Another package we explored was recbole but their requirement of a particular structured data was too much of a headache. Long story short, recommender system libraries need much improvement.

# Deep Cross Network (DCN)

The last hybrid model we explored was a deep cross network. It was developed by Google’s research team and has shown state of the art results in various benchmarks. We believe due to its success and customizable framework that it can possibly outperform our current BiVAE model and have decided to invest our efforts into it.

![Deep Cross Network Architecture ](https://cdn.discordapp.com/attachments/927717200247275561/969647882317746206/unknown.png)


## Embedding Layer

![Embedding Layer Vector Formula ](https://cdn.discordapp.com/attachments/927717200247275561/969647930703228968/unknown.png)

DCN considers input data with sparse and dense features. DCN employs an embedding procedure to transform these sparse features into dense vectors of real values (commonly called embedding vectors). The corresponding embedding matrix for the sparse vectors will be optimized together with other parameters in the network. The combined embedding vectors and dense features are fed as input to the DCN model.


## Cross Network

![Cross Layer Formula ](https://cdn.discordapp.com/attachments/927717200247275561/969648077302550598/unknown.png)

The core of DCN lies in the cross layers that create explicit feature crosses. A feature cross is a synthetic feature formed by multiplying (crossing) two or more features. Crossing combinations of features can provide predictive abilities beyond what those features can provide individually. For an 𝑙-layered cross network, the highest polynomial order is 𝑙 + 1 and the network contains all the feature crosses up to the highest order. Tensorflow has an implementation of cross layers and allows us to build our DCN.

## Deep Network

The cross layers could only reproduce polynomial function classes of bounded degree; any other complex function space could only be approximated. Therefore, a deep neural network is introduced next to complement the modeling of the inherent distribution in the data.


![Hidden Layer Formula ](https://cdn.discordapp.com/attachments/927717200247275561/969648120080248892/unknown.png)

## Combination layer

The input data is in parallel to both the cross and deep networks. Their outputs are then concatenated to create the final input for the output layer. The output layer is a sigmoid fully connected dense layer and outputs our item score vector.

## Adding a Pre-trained Convolutional Neural Network

With the DCN framework’s capability of parallelism, it is possible to implement additional architectures to add to the combination layer. Due to images being unusable as a normal dense feature and their success in our Visual BPR experiments, we plan to add a pre-trained convolutional network in parallel and have our images directly contribute to the item ranking scores.

# Looking Towards the Future

We would like to now outline our plans for the future. We hope to discover the most efficient and best performing pre-trained convolutional neural network for our image feature extraction. We hope to create various baselines for hybrid methods, collaborative filtering methods, and image focused models. We will decide on the best performing features to include in our final model. We will seek to create a better performing model than last quarter using DCN.  Lastly, we hope to deploy our final model into AppFolio’s Computer Science Capstone Team’s website.







