# Update 2

## Recap
Previously, our data contained bee images with bar scales, a QR/Datamatrix code, and other
descriptions related to the specimen.

| ![sample](./16.JPG) |
|:--:|
| <b>Sample Specimen</b>|

The first 3-4 weeks consisted of developing Python scripts that can extract EXIF data from an image file ([MetaData Extractor](https://github.com/harperklauke/Metadata-Extractor)) and renaming the image corresponding to the readings from QR/Datamatrix code ([Universal QR/Datamatrix Rename](https://github.com/booleank/bee-scanner)) .We were able to release these tools as repositories on Github. These repositories are currently being used by two academic institutions. With the initial release, it was pointed out that some requirements may be different on MacOS and Windows so, we made sure to clarify this in the documentation. We hope to address any further issues and clean these tools as they are used throughout the project. 

## Moving Forward
Our new focus is related to the bee itself. As such, we will be working with a different image dataset, which contains close-up, high quality images of bees. Our goal is to create different models that can quantify the number of hair on a given bee image. 

| ![sample](./side-profile.jpg) |
|:--:|
| <b>Lateral View of Bee</b>|

### Researching Different Methodologies
Upon more research on computer vision tools, we found that there were several approaches that we can take to measure the hairiness of bees using raw 2D and 3D image data. Since our group is relatively new to these concepts, we determined that it was best to consult with professionals before taking any drastic steps in our project. Our team reached out to Prof. Yon Visell, an associate professor at UCSB with research in communications and signal processing. As part of our information gathering stage, we looked into the suggestions provided by Yon Visell, such as treating the problem as a block-based processing, using a convolutional neural-network approach, and quantifying bee hair using density. Since the problem will involve unsupervised learning, we sought to try different methods:

Approach 1: Using a supervised learning technique, we may be able to train a random forest classifier to return a binary pixel image, with the hair regions being white. This technique was inspired by the paper FIGARO, HAIR DETECTION AND SEGMENTATION IN THE WILD, where a combination of classic machine learning and computer vision techniques were utilized to get desired results for humans. One drawback to this method is we will be required to preprocess a lot of training images manually and design another model for detecting pixels on where the bee actually is. 

Approach 2: Using methodologies presented in [Hairiness: the missing link between pollinators and pollination](https://peerj.com/articles/2779/), we could try quantifying bee hair using a measure of entropy. The approach is to use an image analysis method to segment the image and compute entropy values for each block.

| ![sample](./fig-1-full.png) |
|:--:|
| <b>Entropy of Bee Face Image</b>|

[Image credit](https://doi.org/10.7717/peerj.2779)
