# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Description

My pipeline consists of 5 steps. First, the images are converted to grayscale. Then, gaussian smoothing is applied to suppress noise and spurious gradients by averaging. The third step is the application of the Canny edge detection algorithm to identify the image boundaries. Afterwards, a focus on the region of interest is achieved by using region masking. Finally, the Hough transform is applied to detect the lines among all boundaries. As a result of this, a set of detected lane lines is shown in the final image: 

[image1]: ./test_images_output/solidYellowLeft.jpg "Detected lines"
![alt text][image1]

The implementation has been performed by using functions of the OpenCV library. Of them, the most important are Canny and HoughLinesP, which provide the Canny detection algorithm and the Hough transform. Additional functions to convert to gray scale or just to represent the calculated lines have also been used.

In order to enhance the visual representation, extrapolation has been implemented in the function draw_lines, so that the full extent of the detected lane line is shown. This has been achieved by splitting the left and right lines through computation of each slope and characterizing the slope and intercept of the two resulting lines. With these results, the characterization has been evaluated at the bottom of the Y-axis and at the top of the defined region of interest. The pair of points obtained for each side are the start and end of our lines:

[image2]: ./test_images_output/solidYellowLeftFull.jpg "Detected lines after extrapolating"
![alt text][image2]

Finally, the pipeline has been tested by using videos instead of images to show its behavior in an environment closer to the reality. Here it was easy to see how the detected lines and the results of the extrapolation strongly depends on the parameters of the canny edge detection algorithm (gradient thresolds), hough transform (number of intersections, number of minimum pixels...) and the sides of the masked region. A hard configuration  of the parameters makes that, for some particular frames, no lines are detected in one side. This can be seen in the file solidYellowLeft.mp4. It is needed, for example, setting the top side of the region of interest at the point 290 of the Y axis, or reducing the min_line_length to 25 to detect at least one dashed line for all the frames. Such a permissive configuration like that, on the other hand, brings to detection of undesired elements that makes the extrapolation less accurate. In order to not disturb the final representation, the initial values of the Canny and Hough parameters have been selected and all calculated lines with a slope lower than or equal to 0.5 have been filtered.

### 2. Potential shortcomings

As previously mentioned, a weakness of the pipeline is that, for some particular frames, no lines are detected in the dashed line. By tuning the parameters of the used algorithms for solving the problem, a calculation less accurate of the lines has been achieved for more frames. Under these circumstances, a filtering of some lines has been implemented. Specifically, all calculated lines with slope lower than or equal to 0.5. In this way only the correct calculation of the lines is shown, but for a tiny number of frames there is no line representation.

### 3. Possible improvements

An enhancement could be applied by calculating the average slopes and intercepts of a number of previous frames. By doing so it would be possible to represent a line for all frames without avoiding a loss of accuracy and the appearance of disturbing lines. The correct performance of this solution would depend on  how many previous frames are used to do the average. The inconvenient of the solution is that additional buffers would have to be defined to store the previous calculations, so the pipeline would maybe need excessive memory resources.
