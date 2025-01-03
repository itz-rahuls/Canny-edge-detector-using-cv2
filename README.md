In this code, we will learn the working of the popular Canny edge detection algorithm developed by John F. Canny in 1986. Usually, in Matlab and OpenCV we use the canny edge detection for many popular tasks in edge detection such as lane detection, sketching, border removal, now we will learn the internal working and implementation of this algorithm from scratch.
 

Theoretical Understanding
The basic steps involved in this algorithm are: 
 

Noise reduction using Gaussian filter 
 
Gradient calculation along the horizontal and vertical axis 
 
Non-Maximum suppression of false edges 
 
Double thresholding for segregating strong and weak edges 
 
Edge tracking by hysteresis
Now let us understand these concepts in detail: 
 

1. Noise reduction using Gaussian filter
This step is of utmost importance in the Canny edge detection. It uses a Gaussian filter for the removal of noise from the image, it is because this noise can be assumed as edges due to sudden intensity change by the edge detector. The sum of the elements in the Gaussian kernel is 1, so, the kernel should be normalized before applying as convolution to the image. In this tutorial, we will use a kernel of size 5 X 5 and sigma = 1.4, which will blur the image and remove the noise from it. The equation for Gaussian filter kernel is
 



 

2. Gradient calculation
When the image is smoothed, the derivatives Ix and Iy are calculated w.r.t x and y axis. It can be implemented by using the Sobel-Feldman kernels convolution with image as given:
 


Sobel Kernels

after applying these kernel we can use the gradient magnitudes and the angle to further process this step. The magnitude and angle can be calculated as
 


Gradient magnitude and angle

 

3. Non-Maximum Suppression
This step aims at reducing the duplicate merging pixels along the edges to make them uneven. For each pixel find two neighbors in the positive and negative gradient directions, supposing that each neighbor occupies the angle of pi /4, and 0 is the direction straight to the right. If the magnitude of the current pixel is greater than the magnitude of the neighbors, nothing changes, otherwise, the magnitude of the current pixel is set to zero.
 

4. Double Thresholding
The gradient magnitudes are compared with two specified threshold values, the first one is lower than the second. The gradients that are smaller than the low threshold value are suppressed, the gradients higher than the high threshold value are marked as strong ones and the corresponding pixels are included in the final edge map. All the rest gradients are marked as weak ones and pixels corresponding to these gradients are considered in the next step.
 

5. Edge Tracking using Hysteresis
Since a weak edge pixel caused by true edges will be connected to a strong edge pixel, pixel W with weak gradient is marked as edge and included in the final edge map if and only if it is involved in the same connected component as some pixel S with strong gradient. In other words, there should be a chain of neighbor weak pixels connecting W and S (the neighbors are 8 pixels around the considered one). We will make up and implement an algorithm that finds all the connected components of the gradient map considering each pixel only once. After that, you can decide which pixels will be included in the final edge map.
Below is the implementation. 
