#**Finding Lane Lines on the Road** 

[//]: # (Image References)

![image0] (./project1_images/solidWhiteRight.jpg "Image Reference")

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I applied gaussian smoothing with ***kernel_size=5*** on the grayscale'd image. 

![image1] (./project1_images/grayscaled-gaussian.png "Grayscaled-Gaussian Smoothing")

I applied canny edge detection on gaussian-smoothed grayscalled image using ***low_threshold=150*** and ***high_threshold=200***. 

![image2] (./project1_images/canny-edge.png "Canny Edge Detection")

The result of canny edge detection image then masked to focus on specific area by creating polygon, this is the region of interest that shows lane line. The region points are ***(0,540), (400,330), (580,330), (960,540)***. 

![image3] (./project1_images/masked-edge.png "Masked Canny Edge")

Then I applied hough transformation on the masked image with ***rho=1, threshold=30, min_line_len=10, and max_line_gap=150***. 

![image4] (./project1_images/hough-transformed.png "Hough Transformed")

The result then drawn into original image using `weighted_img()` function. In order to draw a single line on the left and right lanes, I modified the `draw_lines()` function by increasing the thickness attribute to 10. 

![image5] (./project1_images/result.png "Result")


###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be when the position of camera changed, the lane lines angle of produced image / video will be different. In the pipeline I defined hardcoded region of interest, if the area from different image / video changed then the detection process will be failing i.e it will detect other type of "lines" on the road. 

Another shortcoming could be whenever the road color changed as in challenge task. My pipeline is good to detect lane lines on dark-colored road but when the road color changed to brighter one then it failed to detect the lane lines.


###3. Suggest possible improvements to your pipeline

A possible improvement would be to detect the shape of lane lines in order to adjust initial region of interest, so even though the position of camera changed, or the road condition changed the pipeline would be able to automatically detect correct position of lane lines.

Another potential improvement could be to use different method than canny edge to detect the lane lines on the image so different color of road wouldn't be affecting the pipeline process. Or adding another algorithm before performing canny edge to detect the color changed on the road i.e draw an area of interest few meters in the front of car to detect pixel value, if the color bright then do some process before taking canny edge into account.