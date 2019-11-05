# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals of this project are the following:
* Make a pipeline that finds lane lines on the road under changing light conditions and different lane mark colors.


[//]: # (Image References)

[image1]: ./test_images/solidWhiteRight.jpg "Original Image"
[image2]: ./test_images_output/masked_lines.jpg "Masked Image - White and yellow lanes"
[image3]: ./test_images_output/canny_roi.jpg "Canny Filter applied to ROI"
[image4]: ./test_images_output/hough_lines.jpg "Canny Filter applied to ROI"
[image5]: ./test_images_output/right_left_lane.jpg "A defined right and left lane"
[image6]: ./test_images_output/solidWhiteRight_lines.jpg "Final result"

[video1]: ./test_videos_output/solidWhiteRight.mp4
[video2]: ./test_videos_output/solidYellowLeft.mp4
[video3]: ./test_videos_output/challenge.mp4
---

### Reflection

### 1. Pipeline description.
I divided my pipeline in 3 major steps. The first step is raw data processing. Second step is finding lanes in the preprocessed data and the last step is the visualisation of the data.

#### Step 1: 
Here is the original image:
![alt text][image1]

I converted the image into HSV color space to pick the yellow and white lanes easier. 
![alt text][image2]

After that i reduced the region of interest to the typical area where lanes can possibly be seen by the camera.
By applying a canny filter to the binary image of the lanes the image data is reduced to the edges only.
![alt text][image3]

With the Hough transformation function I find possible lane candidates.
![alt text][image4]

#### Step 2:
My next goal was to distinguish between right and left lanes. To make this decision i use the start and end point of each line which is detected by the Hough transformation and calculate the slope. Lines with negative slopes are possibly right lanes, lines with positive slope possibly left lanes. Until this point there are still several left and right lane candidates. To reduce all candidates down to one left and one right lane I take all start and end points of the "left" Hough lines/lanes and use the polyfit function (degree 1 in this simple case). The same approach is used for the right lines. After this I have one defined right lane and a defined left lane. In case no lanes are detected at all I added exeption handling in this step so the program continues in any case.
![alt text][image5]

#### Step 3:

Both Lanes are finally combined with the original image to show the most likely course of the lanes.
![alt text][image6]

![Watch the complete Video of solidWhiteRight.mp4 here][video1]

![Watch the complete Video solidYellowLeft.mp4 here][video2]


### Video challenge

My pipeline is also able to detect the lanes of the challenge video. At least most of the time. To achieve this, I had to change the hard coded ROI to an image size independent ROI and changed the color picker function to HSV colormap as well.

![Watch the complete Video Challenge.mp4 here][video3]
### 2.  Potential shortcomings with the current pipeline

1. Big changes in the lighting conditions could lead to problems since the color picking approach depends on hard coded color values. The light conditions during cloudy weather or twilight are not shown in any of the test videos. 

2. Each frame is handled individually. Jumps of the detected lanes due to misinterpretation are easily possible.


### 3. Possible improvements to my pipeline

1. The use of histogramm equation could help to handle different lighting conditions.

2. Since one can assume that there are no sudden changes if the lane slope on a normal road one could create a integral element / buffer where the slope of the previous 1 or 2 frames is averaged with the current one. Big jumps of the detected lane caused by misinterpretation could be avoided or at least the effect could be watered down.
