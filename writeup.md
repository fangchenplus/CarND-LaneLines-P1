# **Finding Lane Lines on the Road** 


## Project Target

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # "Image References"

[image1]: ./z.jpg "Yellow line is light and easy to miss"

---

## Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of the following steps.
1. Converter the image to grey scale
2. Using Canny algorithm for edge detection
3. Circle the interesting area by creating a masked image using cv2.fillPoly()
4. Using the Hough transform to find lines from Canny edges
5. Draw the detected lane lines on the image as the output

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by the following steps:
1. Using slope as the criteria to separate the left and right lanes. The reason is the left and right lands will have slope with different signs, i.e., one is positive and the other is negative.
Also, the slope is used as filter to rule out some detected edges that are not lane lines. For example, a horizontal line will have very different slope and is easy to be filtered out. This is especially helpful for the challenge task in this project.
2. After step 1, I have two groups of lines. Then I average the slope and offset within each group as the lane functions are the left and right lines.
3. This step is to extend the lane to make it a continuous line. This is straight forward since I already get the lane function in step 2 and I can just extrapolate. I identified the minimum and maximum y by finding the smallest and largest y in the existing lines, and used them to calculate the corresponding x values. 


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the views of the camera changes a lot. The current algorithm depends on my to assign a fixed area for the lane detection. If the relative position of the lanes regarding to the vehicle changes, it is easy for the lane to be outside of the area, leading to a miss of detection. 

Another shortcoming could be the case when the lines are not very straight. The current assumption is the lines can be approximated as a straight line. However, the lane lines could be curve when the road is straight. In this case, the method may not function well.

Lastly, the current detection relies on the setting of several critical parameters and are sensitive to the environment. When there is shade, or the lane color is light, it is possible to miss the detection and requires re-tuning of the parameters, like the thresholds for edge detection and line identification.

The following picture is one frame from the challenge part that is missed at first. I lowered the edge detection threshold to make the algorithm work with it. But this could potentially make the edge detection more sensitive to unwanted edges.


![alt text][image1]

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to have a better algorithm to calibrate the camera, and ensure the masked area is optimized.

Another potential improvement could be to auto tune the parameters using some machine learning techniques.

The way to combine all the separated line segment in draw_line() can be also improved. The current method is to assign a slope range for left and right lanes. Obviously, this may not work well if the lane slopes change a lot.
