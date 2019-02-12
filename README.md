##The Viola-Jones algorithm
The Viola–Jones object detection framework is the first object detection framework to 
provide competitive object detection rates in real-time proposed in 2001 by Paul Viola 
and Michael Jones. Although it can be trained to detect a variety of object classes, 
it was motivated primarily by the problem of face detection. Designed to detect straight on view.
It's slowly getting surpassed by deep-learning.

1. Creates a grayscale image
2. Starts looking for a face. Checks for certain features. Eyebrows, eyes, nose, mouse. 
Just slides a box along the image.


###Haar-like Features
1. Edge features - thicker than a line, but also drastic color differences. E.g. eyebrow
2. Line features - above and below are different than the line. E.g. mouth
3. Four-rectangle features 

The VJ algorithm is comparing the pixel values to the ideal situation of black and white. 
Takes the average intensity of white pixels. Does the same for black pixels. 
Subtracts one from the other. In an ideal situation the difference between black(1) and white(0) 
pixels is 1. The closer the calculated value is to 1, the closer the real world scenario is.
There are different thresholds. Using training a threshold is identified.


###Integral image
A way to speed up the calculations of the Haar-like features. Each pixel in the original image
has a color value. In an integral image, we're going to have an image of the same size.
However, we're going to calculate all the values to the left and to the top of that pixel, 
including the value of the pixel under view.
In order to get the color values of your box, then you take the calculated value at the 
bottom left(Bl). Then the value above the box on the left (Al), above right (Ar) and outside
bottom right (Br). You then calculate the box's values by Bl - Al + Ar - Br. This excludes
large boxes from the value that are outside of our area of interest. Always have to look at only
4 values, instead of depending on the box size. Haar-like features are easy to calculate using
this method.


###Training classifiers
Shrinks the image to 24 x 24 pixels. Apply the same features in a more limited manner.
Scale the features up when using the real image. Have face images and non-face images 
when training.


### Adaptive boosting 
Even in a small picture the amount of features is enormous. Tries all of the possible positions
of a feature. E.g. edge feature at 2x2, 2x3, 2x4, 4x2, 4x3, 4x5 pixels etc.

`F(x) = a1f1(x) + a2f2(x) + a3f3(x) + ...`

Tests a feature. Finds a feature that matches a lot, but not everything. 
That's one of the weak functions. Then it adds more weight to the false matches and tries
to find another feature that reduces the errors. Adds it as a second function. The functions
are matches that complement each other. Takes the best matching one and then tries to fill
in its shortcomings with additional features. 


### Cascading
We have a sub-window that's sliding along the image. We look for the first feature in our list
of features. Look for the most important feature. If not present, then we reject the window.
If the feature is present, then we check if there's a second feature present. If not present, 
then we reject the image. If present, then check one after another. It's a simplified example.
Actually looks for top 5 features or something like that. Increases feature amount by X after 
every match.