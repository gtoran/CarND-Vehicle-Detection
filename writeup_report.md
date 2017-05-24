##Writeup Template
###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is split between the first two cells IPython notebook. The first cell contains two functions, `get_hog_features` and `extract_features`, which are detailed in Chapter 29 of this lesson. The second cell is where these functions are used in the following workflow:

1. Vehicle and non-vehicle image load.
2. HOG feature extraction from both datasets.
3. Sample output.

I then explored different parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`), settling for `orientation = 9`, `pix_per_cell = 8`, `cell_per_block = 8`.

![HOG](https://gtoran.github.io/repository-assets/CarND-Vehicle-Detection-P5/hog.png)

#### 2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters and found these to be the most responsive, and visually resembling reality in the HOG and YCrCB image versions.

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using the `sklearn` library in the third block of code of the notebook. After extracting features from all images, I ran the training and testing dataset through the `fit()` method to train the classifier. I was able to score a test accuracy of 0.9825.

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I implemented the sliding window search in the eight block of code, which I reused from Udacity's lesson example with some minor tweaks. Mainly, I set default values for most arguments and simplified areas of code which I deemed unnecesary for this project. I also modified the function return so it outputs the rediction results to a list.

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here are some example images of the first stage, when the pipeline has only identified positive matches without discarding false positives, overlaying results, etc.

![Pipeline - Positive Detection](https://gtoran.github.io/repository-assets/CarND-Vehicle-Detection-P5/positive-detection.png)
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)

Here's a [link to my video result](./output_images/project_video.mp4). The end result is very promising!

For reference, here's a [link to my video test](./output_images/test_video.mp4). I've also included a custom short clip which unfortunately didn't identify any vehicles at all. My initial thoughts were that the training dataset is based on US cars only and, although they are very similar, the difference must be just enough to throw off the classifier.

####2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions. I then used the `draw_labeled_bboxes` method which iterates over all detected cars, references them to their label and overlays a box on the final image.

### Here are six frames with their boxes and their corresponding heatmaps:

![Pipeline - Heatmap](https://gtoran.github.io/repository-assets/CarND-Vehicle-Detection-P5/pipeline-heatmap.png)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

My main issue with this project was tweaking the parameters to get it up and running smoothly with the provided testing footage. 

I think potential areas of failure would ultimately depend on the training dataset. In my custom video, it wasn't able to detect a single vehicle (although a couple of false positives seem to show up). 