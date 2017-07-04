Advanced Lane Finding Project
=============================

The steps of this project are the following:

 - Compute the camera calibration matrix and distortion coefficients using a set of chessboard images.
 - Correct the distortion of original images.
 - Use RGB, HLS and magnitude of gradient and combine them to create a threshold binary image.
 - Apply a perspective transform.
 - Detect lane pixels and fit the lane boundary.
 - Determine the curvature of the lane and vehicle position with respect to center.
 - Warp back to original image.
 - Output a video
 - Discussion

Compute the camera calibration matrix and distortion coefficients using a set of chessboard images
-----------------------------------------------------

The code is located in my second cell of my jupyter notebook.

I use **cv2.findChessboardCorners()** to find the cross points on the chessboard and map these points to 
their position in the real world. Then use **cv2.calibrateCamera()**
 to compute the camera matrix and distortion coefficients.
 
Correct the distortion of original images
------------------------------------------

The code is located in my third cell of my jupyter notebook.
    
Use **cv2.undistort()** to correct the distortion. The results are shown below:

![](https://github.com/rainbamboooo/Advanced-Lane-Finding-Udacity-Self-Driving-Car-Nanodegree-Term1-project2/raw/master/1.png)

Use RGB, HLS and magnitude of gradient and combine them to create a threshold binary image
------------------------------------------------------------------------------------------

The code is located in 4-17 cell of my jupyter notebook.

First, I convert the image to HLS mode and test H, L, and S respectively. I find that S channel perform best:

![](https://github.com/rainbamboooo/Advanced-Lane-Finding-Udacity-Self-Driving-Car-Nanodegree-Term1-project2/raw/master/2.png)

Then I test R threshold, L threshold, mag threshold and gray threshold.
Finally, I combine R, L and S threshold together because they perform the best.

Then I use **combine()** function to combine these image together:

![](https://github.com/rainbamboooo/Advanced-Lane-Finding-Udacity-Self-Driving-Car-Nanodegree-Term1-project2/raw/master/3.png)

Apply a perspective transform
-----------------------------

The code is located in the 18 cell in my jupyter notebook.

First, I select four source points that represent the points of a trapezoid formed by lane lines. 
Then I select four destination points and use **cv2. getPerspectiveTransform()** to get the transform matrix 
and **cv2. warpPerspective()** to complete the perspective transform.

![](https://github.com/rainbamboooo/Advanced-Lane-Finding-Udacity-Self-Driving-Car-Nanodegree-Term1-project2/raw/master/4.png)

Detect lane pixels and fit the lane boundary
--------------------------------------------

The code is located in the 19-20 cell of my jupyter notebook.

I use the method of sliding windows search which is provided by Udacity’s courses. 
Plot the histogram of pixel and find two peak. Those two peaks are the pixels of lane lines. 
Then slowly search upwards and detect all the lane pixels. Then use **np.polyfit** to fit the lanes lines.

Determine the curvature of the lane and vehicle position with respect to center
-------------------------------------------------------------------------------

The code is located in the 21-22 cell of my jupyter notebook.

To calculate curvature, I first transform the unit of pixels to the unit of the real world. Then I use the formula of curvature to calculate it.
	
To find the position of the car, I first compute the average sum of the left bottom pixel and the right bottom pixel. Then I use half of the image’s length to subtract it.

Warp back to original image
---------------------------

The code is located in the 22 cell of my jupyter notebook.

I also use **cv2.getPerspectiveTransform** to compute the inverse matrix of transform, then apply it to the warped image.
	
The final output of my test images are:

![](https://github.com/rainbamboooo/Advanced-Lane-Finding-Udacity-Self-Driving-Car-Nanodegree-Term1-project2/raw/master/5.png)

Output a video
--------------

The code is located in the 23- 26 cell of my jupyter notebook.

Discussion
----------

While I was doing this project, I found that my method does not work well when there is shadow on the road. Theoretically, the H channel can solve that problem. However, I tried it myself and failed… Maybe I will try to use some other methods later and improve that problem. I also had problem on selecting source points for a perspective transform because each picture is different. Finally, I chose one that matches all pictures quite close.
