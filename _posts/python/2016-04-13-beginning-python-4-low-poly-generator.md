---
layout: post
comments: false
categories: [python, opencv]
---

OpenCV (Open Source Computer Vision) is a library of programming functions mainly aimed at real-time computer vision, and it is written in C\+\+ and its primary interface is in C++, but it still retains a less comprehensive though extensive older C interface. There are bindings in Python, Java and MATLAB/OCTAVE. The API for these interfaces can be found in the online documentation. (from wiki)

In this post, I'll study how to use these interface for python.

Recently, low poly becomes one popular design style. Here, I'll study OpenCV by making a little program generating low poly images.

The images below are the results of the program.

![result1](/static/img/python/low_poly1.jpg)

![result2](/static/img/python/low_poly2.jpg)

![result3](/static/img/python/low_poly3.jpg)

![result4](/static/img/python/low_poly4.jpg)

#### Attention! The method I used isn't correct strictly. Just for fun.

Install the libraries first:

1. [OpenCV](http://opencv.org/downloads.html)
2. [scipy](http://www.scipy.org/)
3. [numpy](http://www.scipy.org/)

Import the libraries needed.

```
import cv2
import scipy as sp
import numpy as np
import math
from scipy.spatial import Delaunay
```

Read an image and split into three channels. [Here](http://www.opencv.org.cn/opencvdoc/2.3.2/html/modules/highgui/doc/reading_and_writing_images_and_video.html?highlight=imread#cv2.imread) is the document about function imread.

```
img = cv2.imread('source.jpg',3)
b, g, r = cv2.split(img)
h, w = len(img), len(img[0])
```

Then to disassenble the image into multiple polygons(triangles), I uses an algorithm in the library called [SIFT](http://www.opencv.org.cn/opencvdoc/2.3.2/html/modules/features2d/doc/feature_detection_and_description.html?highlight=sift#SIFT) to extract key points which will form the polygons.

```
sift = cv2.SIFT()
kps, des = sift.detectAndCompute(img, None)

points = [[0, 0], [0, h-1], [w-1, 0], [w-1, h-1]]
for kp in kps:
    points.append([int(kp.pt[0]), int(kp.pt[1])])
```

Delaunay algorithm can efficiently realized the Delaunay Triangulation and modeling of the points, so it's used to construct the triangle mesh.

```
tri = Delaunay(points)
for ia, ib, ic in tri.vertices:
	ia, ib, ic = points[ia], points[ib], points[ic]
    # find the points in the triangle ia-ib-ic
	innerPt = get_innerPt(ia, ib, ic) # implement it yourself
    # count the mode color in this triangle
    color = mode_of_color(innerPt, r, g, b)
    for x, y in innerPt:
    	img[y][x] = color

    # draw edges of triangle
    cv2.line(img, (ia[0], ia[1]) , (ib[0], ib[1]), color)
    cv2.line(img, (ib[0], ib[1]) , (ic[0], ic[1]), color)
    cv2.line(img, (ia[0], ia[1]) , (ic[0], ic[1]), color)
```

Finally, saves and shows the images.

```
cv2.imwrite("./out.jpg", img, [int(cv2.IMWRITE_JPEG_QUALITY), 100]) 
cv2.imshow("result", img)
cv2.waitKey()
```

Well, the results look pretty good.