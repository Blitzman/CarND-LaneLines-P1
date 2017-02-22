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
[image2]: ./writeup_images/image1.png "image2"
[image2_result]: ./writeup_images/image2_result.png "image2_result"
[image3]: ./writeup_images/image1.png "image3"
[image3_result]: ./writeup_images/image3_result.png "image3_result"
[image4]: ./writeup_images/image1.png "image4"
[image4_result]: ./writeup_images/image4_result.png "image4_result"
[image5]: ./writeup_images/image1.png "image5"
[image5_result]: ./writeup_images/image5_result.png "image5_result"
[image6]: ./writeup_images/image1.png "image6"
[image6_result]: ./writeup_images/image6_result.png "image6_result"

---

### 1. Basic Pipeline Description

In this section we will describe the minimal basic pipeline that meets the expectations and goals of the project. The pipeline itself is implemented by the function `process_image` and it consists of eight steps. That function expects RGB images as input and will produce RGB images with lane lines overlaid in red.

![Input Image][image1]
![Output Image][image1_result]

#### 1.1. Grayscale Conversion

The first step of the pipeline converts the RGB input to grayscale in order to make it a suitable input for the Canny edge detection step. Using the provided `grayscale` helper function (which makes use of `cv2.cvtColor`) we can easily convert the input image to a single 8-bit intensity channel one.

![Grayscale Conversion][image1_grayscale]

#### 1.2. Intensity Threshold

The second step is a pre-processing one to segment the lanes using an intensity threshold. Notice that this step works too for yellow lanes since their grayscale intensity is close to white and different enough from the road. By segmenting out any pixel whose intensity is below 200 we achieve the following result.

![Intensity Threshold][image1_graythreshold]

#### 1.3. Gaussian Smoothing

Before moving further, we will apply Gaussian smoothing as another pre-processing step to suppress noise and spurious gradients. We found out that a kernel size of 5 is a good choice to smooth over a reasonably good area without being too small or too large.

![Gaussian Smoothing][image1_gaussian]

#### 1.4. Canny Edge Detection

After that, 

![alt text][image1_canny]

#### 1.5. ROI Segmentation

![alt text][image1_roi]

#### 1.6. Hough Line Detection

![alt text][image1_hough]

#### 1.7. Line Fitting

in order to draw a single line on the left and right lanes, i modified the draw_lines() function by ...

![alt text][image1_fit]

#### 1.8. Image Merging

![alt text][image1_result]

#### 1.9. Results

![Input Image][image1]
![Output Image][image1_result]

![Input Image][image2]
![Output Image][image2_result]

![Input Image][image3]
![Output Image][image3_result]

![Input Image][image4]
![Output Image][image4_result]

![Input Image][image5]
![Output Image][image5_result]

![Input Image][image6]
![Output Image][image6_result]

### 2. Improvements on the Basic Pipeline

#### 2.1. Histogram Equalization

#### 2.2. Line Averaging

### 2. Potential Shortcomings in the Pipeline


one potential shortcoming would be what would happen when ... 

another shortcoming could be ...


### 3. Possible Improvements to the Pipeline

a possible improvement would be to ...
