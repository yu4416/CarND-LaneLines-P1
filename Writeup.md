# **Finding Lane Lines on the Road** 

## Writeup 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on the work in a written report


[//]: # (Image References)

[image1]: ./examples/laneLines_thirdPass.jpg "Processed_Example"
[image2]: ./test_images/solidWhiteCurve.jpg
[image3]: ./output_image/grayscale_image_1.jpg
[image4]: ./output_image/blur_image_1.jpg
[image5]: ./output_image/edge_image_1.jpg
[image6]: ./output_image/processed_image_1.jpg
[image7]: ./output_image/processed_image_2.jpg


---

![][image1]

### 1. Pipeline and draw_lines() function description.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I applied a Guassian noise kernel to the image to blur it. And then, the blurred was converted to an edge image by setting a lower and higher threshold. Before applying the hough transform, a region of interest (ROI) was set by selecting 4 vertices to form a trapezoidal region. And the last step was to combine the `draw_lines()` fuction with the `hough_line()` funciton to mark lane lines in images.

![][image2]
> Original Image

![][image3]
> Grayscale Image

![][image4]
> Blurred Image

![][image5]
> Edge Image

![][image6]
> Processed image by the unmodified drawline() function

In order to drew a consecutive single line on the left and right lanes, I modified the `draw_lines()` function by separating line segments by their slopes *((y2-y1)/(x2-x1))*. Note the slope equation was not *((x2-x1)/(y2-y1))*. So, when the slope value was less than 0, it would be considered as left lane. And when slope was greater than 0, it would be considered as right lane. 

![][image7]
> Processed image by the improved drawline() function

Then all the line segments were separated by their slopes, and I applied the `polyfit()` function to fit all left points and right points into 2 lines. The line equations were calculated by the `poly1d()` funciton. Reflected on the image, `cv2.line()` function was used to draw left and right lane lines. 



### 2. Shortcomings with  current pipeline


There were serveral shortcoming wieh my current pipeline. 

First, the pipeline only worked with **RGB** images. If images were in other format like **YUV** or **CMYK**, the current pipeline would not work properly. 

Second, the current pipeline didn't seem to work perfect with the second test video *solidYellowLeft.mp4*. Some glitches were observed within some frames of the processed video. The lane lines drawn on the image were not aligned with the actual lane lines. 

Third, the current pipeline didn't work with curved lane lines. The current `polyfit()` function in `draw_lines()` function only worked with first order polynomial equation. I tried to modify the `polyfit()` function to fit a higher order polynomial equation, but didn't get any luck. So, in the *Optinal Challenge* part, I used the original `draw_line()` function (the unmodified version) to process the video, and wanted to regulate drawn lines by setting a smaller ROI. However, this method didn't work neither. 



### 3. Future improvements to current pipeline

A possible improvement would be to add another function to process images with additional/other color channels. 

Another improvement could be made by modifying  the `draw_line()` function to make the current pipeline more robust with curved lane lines.
