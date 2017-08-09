# **Finding Lane Lines on the Road** 
<img src="final_img/solidWhiteRight.jpg" width="480" alt="Combined Image" />

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project you will detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  

### 1. Pipeline:
The Pipeline consists of  steps:
1. The images consists of yellow and white lines. To Filtered out the colour I converted RGB to HLS colour space.The lines are more 	recognizable in HLS color space
2. Converted the HLS color space image to gray scale (single channel) for edge detection.
3. Before Edge Detection, we need to smoothen the image, to supress the noise. I used Gaussian Blurr with kernel size of 13.
4. To find out the edges of the lanes, I used Canny Edge Detector which uses the pixel gradient values and filters the image according to the Lower and Higher threshold provided. The pixel below lower threshold are ignored, while pixels above higher threshold are accepted. While pixels lying between lower and higher threhold are accepted if there are in connection with the pixels above the higher threshold.The preffered ratio of threshold are 2:1, 3:1.  The threshold are selected dynamically using the median of the gray scale image. the lower and upper threshold are in the range of (0.66*median , 1.33*median )
5. I then selected the Region of Interest with a polygon of trapeziodal shape focusing on the lower part where the probability of finding lane is more. The region outside the ROI is excluded by masking 0.
6. I then used Hough Line Transform to find out the lines in ROI. It returns lines endpoint (x1,y1,x2,y2) detected in the images.
7. We then need to extrapolate the line as a single lane line. For this I used the weighted average method. First I seperated the right and left lane by using slope ( left lane has positive slope and right lane has negative slope). I then weighted average the slope and the intercept by using lenght of the line. After getting the average slope and intercept of Left and Right lane, we just need to extrapolate the lanes.
8.  To draw the lanes on the image, I used the weigthed image of line and the original Image as shown in the Output Directory.    	
![alt text][img]

### 2. potential shortcomings
1.  The ROI needs to be dynamic, as there will be cases when the lanes will not be perfectly fit in the fixed ROI.
2.  The Canny edge threshold needs to be dynamic to get proper edges even when the light intensity varies.
3.  The Hough line Tranform Parameters need to dyanmic to get proper number of lines in every conditions.

### 3. Suggest possible improvements to your pipeline

1.  A possible improvement would be to have dynamic parameters to Canny, Hough functions according to the pixel Intensities. 
2.  Another potential improvement could be to have a dynamic ROI selector by using CNN.
