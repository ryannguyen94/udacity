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

### 1. The pipeline

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I apply Guassian blur with kernel size of 3. Next I use the canny edge detection function to detect edges. Next I define the region of interest for our lane detection and create a mask to turn off everything else outside of the region. Next, I apply Hough transform to find all lines in this masked region. 
In order to draw a single line on the left and right lanes, I modified the draw_lines() function by grouping the lines into 2 group, left and right. The criteria I used for defining which line is on the left is by looking at its x value. If x is smaller than 0.45 the total width of the image, then it's on the left side. Otherwise, if x is larger than 0.55, it's on the right side. Everything else is undefined as I'm not certain which side it belongs to. Next, I create a line by applying a weighted curve-fitting all the data points on the same side, with the weight being the length of the line, so that the ends points of a longer line is more important. Next I calculate the standard deviation of the data set from the curvefit, and then I reject any point that is more than 2 standard deviation away from the rest because they are probably outliers. Then I curvefit one more time with the rest of the data points.

### 2. Shortcomings
The biggest short-coming of my approach is that I force the lane line to be a straightline when it can be a curve. Thus, it doesn't work well with the optional challenge.

### 3. Room for improvement
A possible improvement would be to curvefit this into a curve instead, such as the quadratic function
However, my method to define left and right sides also need to change as curve lines cross expand into the other side of the image.