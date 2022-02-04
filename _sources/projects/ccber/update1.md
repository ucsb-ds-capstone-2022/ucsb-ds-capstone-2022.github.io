# Update 1
## Overview
For the first four weeks, we will be split into 2 groups to work on (T1) Reading barcodes from bee images in a folder and renaming the images with the decoded barcode and (T2) Extract exif data from each image and transferring the data into a tab delimited or comma delimited file (csv)
We will be using a sample size of bee images provided by Dr.Seltman. Each image contains a scale bar, information on locality date, a specimen, and a barcode(unique identifier for the specimen)

![image](./14.JPG)


Our weekly meeting consist of updating everyone on the respective group progress, receiving feedback for improvement, and what to-do for next time. 
Our goal is to have two scripts shared via Github for the Big-Bee project to use, draw feedback, and improve scripts if necessary.

## Progress 
### Week 1
The first meeting (1/12) was spent learning about the Big-Bee project. For instance, Cheadle Center houses the [Natural History Collection](https://www.ccber.ucsb.edu/collections), which is composed of images as well as the physical, preserved specimen. 

Our task moving forward for the next few weeks:

  (T1)Reading barcodes from bee images in a folder and renaming the images with the decoded barcode
  
  (T2)Extract exif data from each image and transferring the data into a tab delimited or comma delimited file (csv)
  
  (T3)Share the scripts via Github for the Big-Bee project to use


### Week 2
#### T1
This week was spent trying to extract the data contained in the QR/Datamatrix codes. We will be using [OpenCV], a library of programming functions primarily used for computer vision. (https://docs.opencv.org/4.x/d9/df8/tutorial_root.html)

Two main packages to read the codes from the images:

[QR Code Reader](https://pypi.org/project/pyzbar/) 

[Data Matrix Reader](https://pypi.org/project/pylibdmtx/)

Two problems when using these packages:

  1.Not every image is able to be read 
  
  2.In the case where the code is able to be read, the runtime is quite long 

Using the code readers alone, only 17/29 images could be read. Upon further research, we learned that some preprocessing is necessary to ensure the QR codes can be read consistently and faster. One example of preprocessing is binarization, which is converting a grayscale image into a black and white image. The [Kraken] (https://github.com/mittagessen/kraken) package includes a binarization function. While the Kraken function did improve the accuracy and speed at which the QR/Datamatrix codes were decoded, it still took a little over 30 minutes to decode the 21/29 images provided. 

For next week, we will try improving the runtime and accuracy by writing a code that will attempt to find the QR/Datamatrix codes within the images, and then read them. 

#### T2
This week, we research different python modules that would be able to extract specific EXIF data from a folder of images. The Pillow ExifTag module proved to be the most efficient as well as the CSV python module.

[Exif Documentation](https://pillow.readthedocs.io/en/stable/reference/ExifTags.html). Using this module we were able to construct a rough python script that iterated through a folder of images and produced a .csv file that contained specific EXIF data. 

We ran into a couple of problems:

1.     The ExifTags module only worked for ‘.JPG’ and not ‘.jpg’ files. So, exif data from only 17/29 images was able to be extracted
2.     The produced csv file contained some data that was not human readable.  
3.     It did not include the corresponding filenames for each image
Despite these issues, the runtime for this code was much faster than other modules that were tested. 

### Week 3
#### T1

This week was spent using the function in the OpenCV library( i.e, threshold, findContours, cvtColor, resize, arcLength, etc.) 
We want to preprocess the image by applying thresholding to isolate pockets of white regions. 
![threshold](./threshold.JPG)

Reasoning: Since QR/Datamatrix codes are a black box shape, we can invert the color(i.e, black to white and white to black), and try to find the white regions. 
For the technique, we used simple thresholding. The parameter of interest is the thresholding value, which converts each pixel on the image to black or white 
depending on the value(i.e, if the thresholding value is 200, any pixel value less than 200 is set to 0, otherwise it is set to maximum value (generally 255). 
Next, we capture these white regions into a rectangular coordinate,which will improve the runtime.

![rectangle](./output_rectangle.JPG)

Previously, the runtime was too long (~30 min) because the decoding function had to iterate through the entire image to find the QR, then decode it. 
Now, the runtime was a lot faster (<2min); however, manual adjustment for the thresholding parameter is required. Since every image may have different lighting conditions in different areas, applying one global, thresholding value may not yield the desired result. 
If we adjust the thresholding parameter for images that could not be processed initially under a preset parameter, we can get 20/29 images to be decoded. 

For next week, we will try experimenting with different thresholding techniques, maintaining a similar/faster runtime. 

#### T2
This week, we focused on ensuring that the EXIF data in the .csv file was human readable. 
Upon further inspection of the documentation, we found that we needed to change some encoding manually. Certain fields with integer-encoded values were transformed by creating an internal dictionary that maps integer keys to their respective ‘readable’ values (i.e. Orientation, ResolutionUnit). Other fields with numeric values were adjusted to the appropriate scale or appended with a specific unit of measurement (i.e. ExposureBiasValue, FNumber). Duplicate tags were omitted and others included as needed.


### Week 4
#### T1
This week was spent on learning and implementing adaptive thresholding. 
We chose adaptive thresholding because unlike simple thresholding, adaptive works well with images with different lighting conditions. 
Adaptive thresholding computes a different thresholding value for each pixel in a region of the image. 
The parameter of interest is the size of the region or pixel neighborhood used to calculate the threshold value. 
From trial-and-error, we determine the size such that the QR/Datamatrix codes stand out as a white square.

After applying some few more transformations, we were able to run the code readers on the pocket of white regions. 

Additionally, a script has been shared on github for user-friendly access. The script will detect barcodes and copy the renamed images into a new folder, while copying unreadable images to another folder and  preserving the original name.

#### T2
This week we tried to tackle the issue that ExifTags module only extracted data from .JPG files. It seemed that the optimal solution to this issue was to do it in the renaming files process. However, we were informed that it was important to stay consistent with how the files were named as it was important to specific institutions to keep “.JPG” or “.jpg”. Therefore, a lot of progress on these changes had to be reevaluated and a different approach was necessary. 
 
Upon more discussion, we found that as we go through certain changes with the images (for example, running the QR code scanner and renaming the files) there may be some significant changes in the EXIF data. Therefore, in the upcoming weeks, we planned to track these possible changes to ensure that no significant information like date taken, resolution, brightness, etc. was altered or even lost. 

### Next Steps 

#### T1
While we got the scanner working for our small sample size, it is not guaranteed to work perfectly, as images vary in lighting and background noise. We hope to receive feedback from users of this project, as well as more samples for testing/updating our code. In order to obtain higher accuracy, we will have to add more ways of handling noise in the images.

With the Exif Data Extractor, we want to ensure that all EXIF data from a folder of images are extracted with their filenames unchanged. In addition, we would like to continue to track any significant changes in EXIF data when changes are made to images and filenames.
