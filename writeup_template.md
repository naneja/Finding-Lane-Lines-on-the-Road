# **Finding Lane Lines on the Road** 
---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:

* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


## (Image References)

![Lanes marked on test images](https://github.com/naneja/finding-lane-lines-on-the-road/blob/master/images/sample.png)
**Lanes marked on test images**


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 

#### Gray Scale
> Returns the image with one channel

#### Gaussian Blur
> Image Smoothening by reducing image noise and detail, 
* https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_filtering/py_filtering.html#gaussian-filtering
    * I Used kernel size of 3  

#### Canny Edge Detection
> https://towardsdatascience.com/canny-edge-detection-step-by-step-in-python-computer-vision-b49c3a2d8123
* I used high_threshold value of 180 (to identify strong pixels) and low_threshold value of 140 (to identify non-relevant pixels)


#### Region of Interest
> Canny Edge Detection provides edges in the whole image. We can mark region of interest to find edges only in the required region.
* I used vertices v1, v2, v3, v4 = (100, 540), (380, 330), (550, 350), (900, 540) to mark rectangular region of interest

#### Hough Lines
> http://homepages.inf.ed.ac.uk/rbf/CVonline/LOCAL_COPIES/MARSHALL/node32.html
> In Hough space, "x vs. y" line is represented as a point in "m vs. b". 
* The Hough Transform is just the conversion from image space to Hough space. So, the characterization of a line in image space will be a single point at the position (m, b) in Hough space.
* A single point in image space has many possible lines that pass through it, but not just any lines, only those with particular combinations of the m and b parameters. 
* A point in image space describes a line in Hough space. So a line in an image is a point in Hough space and a point in an image is a line in Hough space.
* Two points in image space correspond to two lines in Hough Space. 
* Intersection point of the two lines in Hough space correspond to a line in the image space that pass through (x1, y1) and (x2, y2)
* Hough Lines Parameters 
    * rho = 1 
    * theta = np.pi / 180
    * threshold = 2 # min points in image space need to be associated with each line
    * min_line_length = 5 #40
    * max_line_gap = 5 #150 

> Draw Lines
In order to draw smooth lines, first find slope of all Hough Lines and separate Left and Right Lines based on whether the slope is negative or positive
Once the lines are split, the points of the lines can be used to find best fitting curve and drawn over the image



### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when the lane lines are curved. 

Another shortcoming could be sometime the marked lane is at height which should be improved.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to fit the polynomial of higher degree (Although I tried for degree upto 10) or try other algorithm for curved lanes.

