# **Finding Lane Lines on the Road** 

## Writeup 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 9 steps. 

1) Convert the original image to HSV (hue, saturation, value)
2) Isolate the yellow and white pixels on the HSV image (step #1) - using cv2.inRange - and create a mask of these images
3) Convert the original image to grayscale (for easier manipulation later)
4) Combine the greyscale image with the mask from step #2 to highlight the lanelanes on top of the grayscale image
5) Apply Gaussian Blurring on image from step #4 to smoothen edges
6) Apply Canny Edge Detection to smoothened image (step #5) to identify the edges (i.e., sharp changes in pixel values) within the image
7) Isolate region of interest in front of car on image. All other lines from step #6 are removed.
8) Perform a Hough Transform to find lanes within our region of interest (from step #7). These lanes are highlighted in red. 
9) Overlay image from step #8 with original image to create resultant image of road with lanes highlighted in red. 

To draw a single line for the left and right lanes, I modified the draw_lines() function by:
1) Create two arrays - left_points and right_point - to capture potential lines that fall within each lane
2) For each line passed into the function, classify it as belonging to either left or right lane based on its slope. Positive slope is left lane, while negative slope is right lane.
3) For each line, capture the slope and intercept
4) Take the average slope and intercept of lines in each of the left and right lane array
5) Back extrapolate the x-intercept for left and right lanes based on average slope and average intercept of left and right lanes.



### 2. Identify potential shortcomings with your current pipeline


Shortcomings of my pipeline are that: 
1) I have assumes lanes are curved - this will not work for curved roads
2) Lane information 'jumps' from frame-to-frame based on the accuracy of the detector and filter
3) Lanes are presumed to end at the same point (i.e., fixed point in the horizon). This only makes sense for straight roads (as opposed to driving up or down hill)


### 3. Suggest possible improvements to your pipeline

Improvements for my pipeline - based on the shortcomings identified above - are : 
1) Fit non-linear lines 
2) Incorporate lane data from previous frames given videos are time series data
3) Detect the end of the line, rather than presuming a fixed horizon. Possibly by extrapolating the left/right lanes onto the point of horizon (which I'll need to separately detect).