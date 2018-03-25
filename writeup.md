# **Finding Lane Lines on the Road**


## Pipeline

### 1. Grayscale
In a given approach we don not care about the color of the lane, but we're rather going to identify it by contrast.
It means that in terms of color a lane is going to be something light (either white or yellow) on a dark background which is supposedly asphalt.
To achieve it we shall turn everything to grayscale.

I believe (though not sure) that another positive side effect is that grayscale lowers the number of possible color values to 1 bit, which possibly makes further computations faster.  

### 2. Blur
Objects which we're after are not only light but also relatively large and continuous. In order to filter out smaller objects scattered on the asphalt, we will blur them out.
The idea is that each and every pixel will be 'moved' closer to it's neighbours in regards of color, not the location of course.
Hence the small objects will be 'merged' by the dark background surrounding them, while large objects will retain due to the fact that in this case neighbour pixels are going to be of the same color.

### 3. Canny Edge Detection
Now that we have a blurred grayscale image we can apply the next technique, which is Edge detection.
The approach here is that the algotirhm is measuring a derivative of pixels color values.
In human words it essentially means that it measuers how drastically does a given pixel differs from it's neighbors.
If the difference is within a given treshold, we 'capture' the pixel.

As a result we'll have only those pixels, which are on the border of areas with drastically different colours i.e. 'edges'.
In the next steps we will be drawing lines according to these pixels and our Region of interest

### 4. Region of interest
It is a trapezoid in the bottom and centre, which most probably contains the lines of the lanes. 

### 5. Hough lines transformation
This is the technique to transform the repsresentation of a line into a point in a different space, which is named after it's inverntor.


A possible space may be m = f(b), where m and b are parameters of a line representation y = mx + b.

Another option is r = f(theta), where p and theta are parameters of a line representation r = x * cos(theta) + y * sin(theta)

Interestingly, a point in linear space is represented as a line in Hough space, which makes sense, because there's a number of lines, which a point can belong to.
Thus for a given number of points in linear space we get a number of lines in Hough space. Those lines, which intersect in Hough space represent a line, going through the corresponding points.

The algorithm provided by OpenCV is transforming points in a given Region of Interest into lines in Hough space, and searches for their intersection with  

### 6. Lines extrapolation
Given a set of lines out of the previous step, we need to average them into one line per each lane.
To do so, we will implement the following steps:
- Calculate slope and intercept for each line, where:

Slope = (y2-y1)/(x2-x1);

Intercept = y1 - slope(x1,y1,x2,y2) * x1.

The latter derives from the line representation y = mx + b i.e. y = Slope * x + Intercept

- Then we define a slope treshold, according to which we will filter the lines and sort them into positive and negative.
Output of this step is two collections of lines with positive and negative slopes i.e. left and right inclined lines.

- We calculate an average slope for each collection
- Given that we know y coordinate of our region of interest, what we need is to calcuate x coordinates, which is straightfoward if we know the slopes and intercepts:

x = y - intercept) / slope

Thus, we calculate x coordinates for bottom and tompmost y coordinates for left and right inclined lines.

Profit.



### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

## Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I ....

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image:

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ...

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...