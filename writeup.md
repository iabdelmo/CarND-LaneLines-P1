# **Finding Lane Lines on the Road** 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* The Pipeline shall be able to work over images or videos 

---

### Reflection

### 1. Describtion of the Pipeline

My pipeline consisted of 5 stages:

1. Edge detection stage; In this stage the pipeline will convert the image/video frame to gray scale then using the canny edge detction 
it will detect the image edges.

2.Applying ROI "Region of Interset" stage; In this stage the pipeline will have a pre-calculated polygon that reflects the ROI region in the 
processed image. The input of this stage is the output of the edge detection stage and the output will be an image with detected edges 
in the RIO area.

3.Hough transform stage; In this stage using the hough transform algo. the pipeline will take the output of ROI stage and try to construct lines from the edge points. 

4. Belending the detected lane lines with original stage; In this stage the pipeline will belend the detected lane lines in the hough transform stage with the original image.

5. Identifying the full extent of the detected lane lines stage; In this stage the pipeline will take the output of the 3rd stage and produce a full extended lane line start from the bottom of the image to the top in the ROI area.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function to examine the slope value , slope direction and X-axis position for each line detected in the Hough transform stage to assign the line to it's corresponding lane segment "Right Lane segment or Left Lane segment".

The slope threshold, slope direction and the X-axis Threshold are pre-calculated using the below strategy:

  1.The slope threshold values are calculated by defining a threshold angle range for the lines 20 < line angle > 45 degree, So the lines out of this range will be neglected and not assigned to any segment.
  
  2.The slope direction means, If the slope is negative so it belongs to the left lane segment and if it is positive so it belongs to the right lane segment. 
  
3.The X-threshold is the middle of the image which is 500, So if the line x coordinates > 500 so the line is belonging to the right lane segment and if the x < 500 so the line is belonging to the left lane segment.

Using these three threshold combined together the pipeline can assigned the detected lines to their corresponding lane segment.

In the 5th stage of the pipeline, It takes the left lane seg. list and the right lane seg. list and calculates the fitting coefficients 
"m and b". Then extrapolate x points in the bottom/top of the image for the left side/right side using the fitting coefficient for each side.

Finally, The pipeline has the ability to return only one output form one the pipeline stages or All of the stages output by an argument can be specified by the caller of the pipeline.  

### 2. Identify potential shortcomings with your current pipeline

1. The ROI is pre-calculated so if the mounting pos. of the camera is changed this will a wrong output results from the pipeline. To overcome the pipeline should have an automatically way to calculate the ROI polygon.

2. The used technique for classifying the detected line belongs to which lane segment also pre-calc. and this will lead to a wrong results for the full extend lane lines output if the image/video has a lines with angles out of the defined angle threshold and this what I have experienced with the challenge video. 

This could be fixed by making the slope threshold automatically calculated, By getting the average of the slope/lane then get its maximum and minimum values and using them we can define the slope threshold for examining the lines lane.

### 3. Suggest possible improvements to your pipeline

1. Automatical way to calculate the ROI polygon.

2. Automatical way to calculate the slope thresholds.

