# Advanced Lane Lines

The goal of this project is to detect lane lines more accurately and find the radius of curvature.

## Steps of the project
* Compute the camera caliberation matrix and undistort camera images.
* Use Color Transforms and Gradient Transform to convert the image into a thresholded binary image.
* Apply perspective transform to get a Birds-Eye og the pre-processed image.
* Calculate the Quadratic Equations which fits the given left and right lane lines.
* Calculate the curvature of left and right lanes.
* Un-Warp the lane line's and project on the orginal image.


[//]: # (Image References)

[image1]: ./output_images/Distorted.jpg
[image2]: ./output_images/Undistorted.jpg
[image3]: ./output_images/Straight_lines_1.jpg
[image4]: ./output_images/Undistorted_Straight_lines_1.jpg


## Rubic Points
** **
#### Writeup / README
**The writeup / README should include a statement and supporting figures / images that explain how each rubric item was addressed, and specifically where in the code each step was handled : **

Yes.

#### Camera Caliberation

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]![alt_test][image2]

#### Pipeline
**Provide an example of a distortion-corrected image.**

Here is an example of Image before distortion correcion and after distortion correction.
![alt text][image3]![alt text][image4]