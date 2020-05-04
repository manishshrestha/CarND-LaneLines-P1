# **Finding Lane Lines on the Road** 

## Writeup 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./sample/edges.jpg "edges"
[image3]: ./sample/gray.jpg "gray"
[image4]: ./sample/with_hough_lines.jpg "with_hough_lines"
[image5]: ./sample/blur_gray.jpg "blur_gray"
[image6]: ./sample/final_images_w_lines.jpg "final_images_w_lines"
[image7]: ./sample/orginal.jpg "orginal"
[image8]: ./sample/masked_edges.jpg "masked_edges"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps. 

a. First, I converted the images to grayscale, 
![alt text][image3]

b. Then I applied Gaussion blur filter with kernel size of 5 to the grayscale image, in order to smooth out the grayscale
image
![alt text][image5]

c. Then Canny edge detection was done on the smooth image with low threshold of 100 and high threshold of 150.
![alt text][image2]

d. Then only the region of interest which is a polygon (nearly triangular in shape) in front up to the horizon from the 
image bottom is considered for further processing. 
![alt text][image8]

e. Then using Hough transform, the lines are detected in the region of interest
![alt text][image4]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function.  The slope and 
intercept for each of the lines were calculated. Some extreme and unexpected slopes were removed. The lines belonging to 
left or right lanes were collected in different list as a tuple of slope and intercept. The lines that had positive 
slopes are classified to be belonging to right lane line and the other lines with slope of 0 or less were considered to
belong to left lane line. Then the slopes and intercept for each of the lane line were averaged, to get the average 
slope and average intercept. Since the lane lines can be supposed to be going from the bottom of the image to the
horizon, the y positions of the lane lines were taken to be height of the image (which represent the bottom of the image)
and 3/5 of the height of the image respectively. Using the averaged slope and intercept, the x position of both the ends
of each of the lane lines were calculated. Doing this, the x and y coordinates of each end point of the both the lane 
lines were found and drawn.  Another things to mention are:
  * in some cases, the line are not detected on either of both of the lanes, then in such case I have opted not to 
  display any lane line in that particular side. Other option could have been to draw a lane line from the previous 
  detected lane. 
  * the line with slowing rising slope (i.e. <0.5) were discarded, since the lane lines would normally be not that 
  horizontal from camera view and we can eliminate some line due to the car  or objects on the road.
  * For the Optional challenge and since in my view, the lane lines should be converging to the center of the front
   view of the image,  and  in order to eliminate some noisy (false positive) lines that are nearly vertical, the lines 
   with slope greater than 80 degree with horizon was removed. I do not know whether this approach for handling this 
   optional challenge is correct and let me know if there are other ways to improve for this optional challenge.


f. Finally the lines detected from Hough transform are superimposed on the original image, to show the final output of 
our lane line detection pipeline
![alt text][image6]





### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when if the weather condition is rainy or snowy. Even in the 
similar climate, there could be some possibility that the lane lines are not detected. May be the tuned parameter for 
the algorithms used or the algorithms themselves in the pipeline, are not good enough to detect all the line. In one case
 where there was yellow lane line on the left, it could not be detected. Also I have assumed that the lane lines cannot 
 be very slowly moving (nearly horizontal) or very steep (nearly vertical), but if it is really the case, due to some 
 orientation of the vehicle with respect to the road while maneuvering from side to side and making some angle to the 
 road, then the assumption holds false and can mislead further decisions based on it. Also what if there are other vehicles or 
 living beings in front of or sideways, blocking the lane lines, then the detection would go wrong. 



### 3. Suggest possible improvements to your pipeline

A possible improvement would be to research on better algorithms for the pipeline and to test extensively and tune 
further the approved algorithms for more robustness.  
