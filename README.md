# Vehicle Detection and Tracking
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

---

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./examples/car_not_car.png
[image2]: ./examples/car_not_car_hog.png
[image3]: ./examples/sliding_windows.jpg
[image4]: ./examples/heatmap1.png
[image5]: ./examples/heatmap2.png
[image6]: ./output_images/frame_1.jpg
[image7]: ./examples/heatmap_39.jpg
[image8]: ./examples/output_39.jpg
[image9]: ./vimeo_link.png "Link to Vimeo"
[video1]: ./output_videos/project_video.mp4

---

[![Vimeo Link][image9]](https://vimeo.com/236369618)

---
### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

> The code for this step is contained in the second code cell of the IPython notebook in the function `get_hog_features()`.  

> I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

> ![Car and Non-Car Image][image1]

> I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

> Here is an example using the `YCrCb` color space and HOG parameters of `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:

> ![Car and Non-Car HOG][image2]

#### 2. Explain how you settled on your final choice of HOG parameters.

> I tried various combinations of parameters and settled with parameters that gave the best training accuracy.

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

> I trained a linear SVM using all the available features which included Histogram of Oriented Gradients (HOG), Spatial Color Features and Histogram of Color Features. This can be seen in the third code cell of the IPython notebook.

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

> I decided to search all window positions at a scale of 1.5 over the bottom region of the image and came up with this:

> ![Sliding Windows][image3]

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

> Ultimately I searched on one scale using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here are some example images:

> ![Heatmap #1][image4]
> ![Heatmap #2][image5]
> ![Sample Frame][image6]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
> Here's a [link to my video result](./output_videos/project_video.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

> I recorded the positions of positive detections in each frame of the video. I stored the history of positive detections for last 8 frames and used that to create a heatmap and then thresholded that map to identify vehicle positions. I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap. I then assumed each blob corresponded to a vehicle. I constructed bounding boxes to cover the area of each blob detected. 

> Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here is the heatmap for last frame:
> ![Heatmap][image7]

### Here the resulting bounding boxes are drawn onto the last frame in the series:
> ![Bounding Box][image8]

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

> I followed the lesson to create my pipeline. The tweak I made to get more stable bounding boxes was to store the history of bounding boxes for last eight frames and use that to generate the heatmap. Then used a larger threshold value for the heatmap which automatically got rid of all false positives which appeared in intermittent frames.

