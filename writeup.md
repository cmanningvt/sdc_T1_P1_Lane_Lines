# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images/solidWhiteCurve.jpg "solidWhiteCurve"
[image2]: ./test_images_output/solidWhiteCurve.jpg "solidWhiteCurve_output"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 

1. I converted the images/frames to grayscale.
2. I blurred the image/frame using a gaussian blur with a kernal size of 3.
3. I ran Canny edge detection with low and high thresholds of 75 and 150, respectively.
4. I applied a region of interest the the output of the Canny edge detection algorithm. This region was a trapezoid originating the bottom corners of the image/frame and terminating near the center of the image/frame.
5. I ran a hough transform. The outputs of the hough transform were checked for validity (i.e. position and slope) and then potentially filtered with lines from previous frames if the file was a video file. The filtering was done through a weighted running average where the most recent values had more weight than older values.
6. I drew final lines on the image/frame. 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by iterating through all elements of the Hough transform output. For each line I would calculate the slope and intercept. If the slope was invalid (i.e. 0, inf, or out of range of my bounds), then the line was ignored. Lines would also be separated into left and right lane lines by their slope.
After removing and separating all lines, I averaged all lines that fell in the left lane lines category. This was repeated for right lane lines.
If the input file was a video, then I would account for any previously drawn lines and I would use a weighted moving average to smooth the lines. This method proved to be very effective.
Lastly, I would draw the lines on the image/frame and updated the line history for use in the next frame.


Here is one example of my pipeline operating on an image.
![alt text][image1] 
![alt text][image2]



### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming of my current pipeline is handling of different lighting conditions. I have not tested at all in dark
or poorly lit areas.

I also use a global variable to accomplish my filtering and smoothing of lane lines. There are cleaner ways to accomplish this task without global variables.


### 3. Suggest possible improvements to your pipeline

There are three possible improvements that I would like to implement in the future.

a. The first is the ability to recognize curved lane lines with better precision. 
This would take a few possible steps including tuning the Hough function output to recognize lines that are not long and straight.
If you can tune the Hough transform to recognize shorter segments of lines, then you can use all of those lines to form a curve.
This can be done by looking at all of the end points and finding a polynomial fit for the lane line. Then use cv2.line to draw many 
short line segments. Of course, with this approach, filtering/smoothing the lane lines could possible become a bigger challenge, as
well as the region of interest.

b. Another potential improvement is to have a dynamic region of interest. For example, in the the videos, the horizon was generally
at different points in the frame. Having an algorithm that recognizes the horizon and modifies the region of interest to match this
would be extremely beneficial.

c. Restructuing the code to remove the dependency on global variables right now.


### 4. Some issues that I ran into along the way

1. Using a Jupyter notebook for the first time. I find this a very interesting way to share code though now and am looking forward to using them more in the future.

2. Understanding the fl_image function and how to implement that with my pipeline. For a while, I did not understand that this would iterate through each frame individually.

3. I would frequently recieve OS Errors when running my code to read the video files. I still am not able to run my Jupyter notebook fully without it crashing while reading one of the video files. I can successfully operate on any vidoes individually, but need to restart the kernal sometimes to be able to run all 3 videos successively. Any input would be appreciated as I am sure this is something that may be an issue in future projects as well.
