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

### 3. Canny Edges Detection
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


## Reflection

### 1. Modification of  draw_lines() function.

I've implemented the flow, described above in point 6.


### 2. Identify potential shortcomings with your current pipeline and suggest solutions

* Grayscale
  * **Problem.** I can think of various surface and lighting conditions when the lanes and the road itself would be of the same shade after grayscaling, while still of different colors originally.
  * **Solution.** I believe there's a way to differentiate lanes by colors. Maybe instead of graysclaing we can play with various color models
* Blur
  * **Problem.** Not sure if it's a problem of the pipeline itself, though, lane can be poorly supported and partly erased, so that it won't be a solid line, but scattered pieces of it. In this case blur might erase these pieces.
  * **Solution.** Didn't check my idea, though I can think of a pipeline, when we can detect clusters by color and location, then apply linear regression for each of them, and filter out by a certain RMSE treshold. Those which will have low error are going to be lines. Not sure if it will work though.    
* Canny Edges Detection
  * **Problem.** What I can think of is that in different lighting conditions the treshold might be adjusted in order to be sensitive enough
  * **Solution.** Again, it is just an idea, though we can measure the derivatives of pixels' colors across the image, get it's distribution, and the take the min and max values of the derivative in a given range like Mean +/- Sigma. Thus we'll get a lighting-sensitive tresholds.   
* Region of interest
  * **Problem.** It is fixed. Road can bend left, right or even downhill, and the good part of visible lanes will be out of our view.
  * **Solution.** I am not quite familiar with deep learning yet, though I intuitively feel that there's a way to train a classifier to detect the exact area containing lanes.  
* Hough lines transformation
  * **Possible improvements.** I believe, there's a way to detect curved lines, not only straight. The direction I'm thinking towards is that we take the representation of a line, and build another space out of it's parameters. Nothing prevents us to play with representations of arcs, so that we could transform an arc into a point, and point into a number of lines. Those lines, which intersect in Hough space make the points of an arc in linear space.
* Lines extrapolation
  * **Problem.** My implementation is very naive, and works only with straight lines
  * **Solution.** Research towards detecting curved lines on the previous step, then research on how to represent each line as a vector of parameters, and then average those vectors.