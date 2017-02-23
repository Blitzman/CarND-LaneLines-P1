#**Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

**finding lane lines on the road**

the goals / steps of this project are the following:
* make a pipeline that finds lane lines on the road
* reflect on your work in a written report


[//]: # (image references)

[image1]: ./writeup_images/image1.png "image1"
[image1_grayscale]: ./writeup_images/image1_grayscale.png "image1_grayscale"
[image1_graythreshold]: ./writeup_images/image1_graythreshold.png "image1_graythreshold"
[image1_gaussian]: ./writeup_images/image1_gaussian.png "image1_gaussian"
[image1_canny]: ./writeup_images/image1_canny.png "image1_canny"
[image1_roi]: ./writeup_images/image1_roi.png "image1_roi"
[image1_hough]: ./writeup_images/image1_hough.png "image1_hough"
[image1_fit]: ./writeup_images/image1_fit.png "image1_fit"
[image1_result]: ./writeup_images/image1_result.png "image1_result"
[image2]: ./writeup_images/image2.png "image2"
[image2_result]: ./writeup_images/image2_result.png "image2_result"
[image3]: ./writeup_images/image3.png "image3"
[image3_result]: ./writeup_images/image3_result.png "image3_result"
[image4]: ./writeup_images/image4.png "image4"
[image4_result]: ./writeup_images/image4_result.png "image4_result"
[image5]: ./writeup_images/image5.png "image5"
[image5_result]: ./writeup_images/image5_result.png "image5_result"
[image6]: ./writeup_images/image6.png "image6"
[image6_result]: ./writeup_images/image6_result.png "image6_result"

---

### 1. Basic Pipeline Description

In this section we will describe the minimal basic pipeline that meets the expectations and goals of the project. The pipeline itself is implemented by the function `process_image` and it consists of eight steps. That function expects RGB images as input and will produce RGB images with lane lines overlaid in red.

![Input Image][image1]
![Output Image][image1_result]

#### 1.1. Grayscale Conversion

The first step of the pipeline converts the RGB input to grayscale in order to make it a suitable input for the Canny edge detection step. Using the provided `grayscale()` helper function (which makes use of `cv2.cvtColor`) we can easily convert the input image to a single 8-bit intensity channel one.

![Grayscale Conversion][image1_grayscale]

#### 1.2. Intensity Threshold

The second step is a pre-processing one to segment the lanes using an intensity threshold. Notice that this step works too for yellow lanes since their grayscale intensity is close to white and different enough from the road. We used a helper function `intensity_threshold()` which sets the intensity of any pixel below a specified threshold to 0. By segmenting out any pixel whose intensity is below 200 we achieve the following result.

![Intensity Threshold][image1_graythreshold]

#### 1.3. Gaussian Smoothing

Before moving further, we will apply Gaussian smoothing as another pre-processing step to suppress noise and spurious gradients. We found out that a kernel size of 5 is a good choice to smooth over a reasonably good area without being too small or too large. By using the provided `gaussian_blur()` function we get this result.

![Gaussian Smoothing][image1_gaussian]

#### 1.4. Canny Edge Detection

After that, we applied the Canny edge detector to generate a binary image to trace out the edges. We used the `canny()` helper function with a `low_threshold` of 50 and a `high_threshold` of 150. Maintaining the recommended 1:3 ratio we were able to achieve good detection results as shown in the following image.

![Canny Edge Detection][image1_canny]

#### 1.5. ROI Segmentation

Next, we will filter any edges detected around the periphery that aren't lane lines. In order to do so, we applied a ROI segmentation process, assuming that the lanes will always appear in roughly the same region of the image of the front facing camera. By using the provided helper function `region_of_interest()` we apply a mask to keep a region of the image defined by a polygon formed from a set of vertices, the rest of the image is set to 0 intensity. We used the following vertices `[[(0,imshape[0]),(450, 320), (490, 320), (imshape[1],imshape[0])]]` to achieve a reasonably good result.

![ROI Segmentation][image1_roi]

#### 1.6. Hough Line Detection

Then we operated on those segmented edges to generate an array of line segments using the Hough transform operation. The helper function `hough_lines()` provided us with the needed OpenCV functionality to apply that operation. After testing various configurations we found out that the set of parameters `rho = 4`, `theta = np.pi/180`, `threshold = 10`, `min_line_length = 8`, and `max_line_gap = 16` produces adequate results.

![Hough Line Detection][image1_hough]

#### 1.7. Line Fitting

In order to draw a single line on the left and right lanes, we modified the `draw_lines()` to divide the lines into two arrays: one for the left lines and another one for the right ones. Lines are divided based on their X starting point -- if it falls on the left half of the image it will correspond to a left line, otherwise it will correspond to a right one -- and then each array is used to fit a line using `np.poly1d` and `np.polyfit`. As a result, a fitted line is generated for the left lane `f_l` and for the right lane `f_r` separately.

![Line Fitting][image1_fit]

#### 1.8. Image Merging

At last, the input image is combined with the fitted lines to generate the annotated output using the helper function `weighted_img()`.

![Image Merging][image1_result]

#### 1.9. Results

This pipeline was tested on the provided test images, producing in all cases reasonably good results (including those images with yellow lanes).

![Input Image 1][image1]
![Output Image 1][image1_result]

![Input Image 2][image2]
![Output Image 2][image2_result]

![Input Image 3][image3]
![Output Image 3][image3_result]

![Input Image 4][image4]
![Output Image 4][image4_result]

![Input Image 5][image5]
![Output Image 5][image5_result]

![Input Image 6][image6]
![Output Image 6][image6_result]

It was also successfully tested on the provided videos: white and yellow (click to watch).

[![White Basic](http://img.youtube.com/vi/pvyCv6XrOyY/0.jpg)](http://www.youtube.com/watch?v=pvyCv6XrOyY "Self-Driving Car Nanodegree - P1: Finding Lane Lines - White Basic")
[![White Basic](http://img.youtube.com/vi/qAYoS7JaMnk/0.jpg)](http://www.youtube.com/watch?v=qAYoS7JaMnk "Self-Driving Car Nanodegree - P1: Finding Lane Lines - Yellow Basic")

### 2. Potential Shortcomings in the Pipeline

one potential shortcoming would be what would happen when ... 

* Yellow lines
* Road surface color variations
* Shaky lines
* Curves

another shortcoming could be ...

### 3. Possible Improvements to the Pipeline

a possible improvement would be to ...

### 4. Implemented Improvements on the Basic Pipeline

#### 4.1. Histogram Equalization

#### 4.2. Yellow and White Masks

#### 4.3. Outlier Filtering

#### 4.4. Line Averaging

#### 4.5. Curved ROI
