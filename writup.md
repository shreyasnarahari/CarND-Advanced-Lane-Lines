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

[image1]: ./output_images/Distorted.jpg "image1"
[image2]: ./output_images/Undistorted.jpg "image2"
[image3]: ./output_images/Straight_lines_1.jpg
[image4]: ./output_images/Undistorted_Straight_lines_1.jpg
[image5]: ./output_images/Binary_Image_0.jpg
[image6]: ./output_images/Binary_Image_1.jpg
[image7]: ./output_images/Binary_Image_2.jpg
[image8]: ./output_images/Binary_Image_3.jpg
[image9]: ./output_images/Binary_Image_4.jpg
[image10]: ./output_images/Binary_Image_5.jpg
[image11]: ./output_images/Binary_Image_6.jpg
[image12]: ./output_images/Binary_Image_7.jpg
[image13]: ./output_images/Perspective_Image_0.jpg
[image14]: ./output_images/Perspective_Image_3.jpg
[image15]: ./output_images/Perspective_Image_7.jpg
[image16]: ./output_images/Sliding_Window_0.jpg
[image17]: ./output_images/Sliding_Window_3.jpg
[image18]: ./output_images/Sliding_Window_7.jpg
[image19]: ./output_images/Final_Images_0.jpg
[image20]: ./output_images/Final_Images_1.jpg
[image21]: ./output_images/Final_Images_2.jpg
[image22]: ./output_images/Final_Images_3.jpg
[image23]: ./output_images/Final_Images_4.jpg
[image24]: ./output_images/Final_Images_5.jpg
[image25]: ./output_images/Final_Images_6.jpg
[image26]: ./output_images/Final_Images_7.jpg

## Rubic Points

#### Writeup / README
**The writeup / README should include a statement and supporting figures / images that explain how each rubric item was addressed, and specifically where in the code each step was handled :**

Yes.

#### Camera Caliberation

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1] ![alt_test][image2]

#### Pipeline
**Provide an example of a distortion-corrected image.**

Here is an example of Image before distortion correcion and after distortion correction.
![alt text][image3] ![alt text][image4]

**Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image. Provide an example of a binary image result.**

* The S-Channel in HLS Color Space was chosen with limits as (170, 255).
* Sobel Magnitude with limits (80,255).
* Yellow Mask was used by converting the image to HSV Color Space and then applying limits to each channel individually as H -> (0,80) , S -> (100,255) , V -> (100,255). This mask helps us to detect the yellow lines in images.
* White Mask was also used, the image was retained in RGB Color Space and then limits were applied to each channel individually as R -> (180,255) , G -> (180,255) , B -> (180,255)
* All the above transforms are merger to form one single binary image.

The code for this has been clearly mentioned in the IPython Notebook.

![alt text][image5]
![alt text][image6]
![alt text][image7]
![alt text][image8]
![alt text][image9]
![alt text][image10]
![alt text][image11]
![alt text][image12]

**Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.**

The points were chosen on the original image to represent a approximate rectangle when transformed. The coordinates chosen were:
* `left bottom = [0,720]`
* `right bottom = [1280,720]`
* `left top = [557, 460]`
* `roght top = [732, 460]`

The points chosen for after the transformation were:
* `left bottom = [200, 720]`
* `right bootom = [1080, 720]`
* `left top = [200, 0]`
* `right top = [1080, 0]`

![alt text][image13]
![alt text][image14]
![alt text][image15]

**Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?**

As we can see in the previous example the histogram is used to choose the point to start the sliding window search.
The coordinates of the quadratic equations(Both for left and Right) are passed through a queue to smoothen out any jumping in the lines. The smoothing happens over 15 frames of information.

![alt text][image16]
![alt text][image17]
![alt text][image18]

**Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.**

Here are results of the pipeline:

![alt text][image19]
![alt text][image20]
![alt text][image21]
![alt text][image22]
![alt text][image23]
![alt text][image24]
![alt text][image25]
![alt text][image26]

#### Pipeline (video)

**Provide a link to your final video output. Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).**

The Video is available in the project repository.

#### Discussion

**Briefly discuss any problems / issues you faced in your implementation of this project. Where will your pipeline likely fail? What could you do to make it more robust?**

There are a lot of problems and drawbacks in this pipeline:
* One of the main being Shadows, once the pipeline encounters the whole road covered in shadow like in the challenge video the pipeline fails to identify the lane lines properly. To overcome this challenge I used techniques like CLAHE and Histogram Equilization but saw no improvement as this let a lot more noise come into the pipeline which made detection of lanes even more difficult. 
* This model also fails under sharp curves which ment only a part of the lane was being transformed properly which lead to much less information for marking the lanes.
*  The pipeline is also sensitive to parameters and is not robust to changes.
*  I think a deep learning approach would detect the lane lines much better and in a robust way.
