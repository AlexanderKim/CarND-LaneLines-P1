# **Finding Lane Lines on the Road** 


## Pipeline 

### 1. Grayscale
In a given approach we don not care about the color of the lane, but we're rather going to identify it by contrast.
It means that in terms of color a lane is going to be something light (either white or yellow) on a dark background which is supposedly asphalt.
To achieve it we shall turn everything to grayscale.

### 2. Blur

### 3. Canny Edge Detection

### 4. Region of interest
### 5. Hough lines transformation
### 6. Lines extrapolation

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
