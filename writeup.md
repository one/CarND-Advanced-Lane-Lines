## **Advanced Lane Finding**

### Writeup by Hannes Bergler
---

**Advanced Lane Finding Project**

The goals / steps of this project were the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./output_images/writeup_images/undistort.png "Undistorted"

[image2]: ./output_images/writeup_images/test4_original.jpg "Original Image"
[image3]: ./output_images/writeup_images/test4_undistorted.jpg "Undistorted Image"
[image4]: ./output_images/writeup_images/test4_binary.jpg "Binary Image"
[image5]: ./output_images/writeup_images/test4_binary_masked.jpg "Binary Masked Image"
[image6]: ./output_images/writeup_images/test4_binary_warped.jpg "Binary Warped Image"
[image7]: ./output_images/writeup_images/test4_warped_lines.jpg "Warped Image With Detected Lines"
[image8]: ./output_images/writeup_images/test4_final_output.jpg "Final Output Image"

[video1]: ./output_videos/project_video_out.mp4 "Video1"
[video2]: ./output_videos/challenge_video_out.mp4 "Video2"


## Rubric Points
#### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/571/view) individually and describe how I addressed each point in my implementation. 
---

### I. Submission Files

My project includes the following files:
* writeup.md - summarizing the project results (You're reading it!)
* Advanced_Lane_Finding_P4.ipynb - Jupyter notebook containing the project's python code
* output_images - folder containing all output images
* output_videos - folder containing the output videos


### II. Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the second code cell of the Jupyter notebook `Advanced_Lane_Finding_P4.ipynb` located in the project's base directory.

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![Undistorted][image1]


### III. Pipeline

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![Original Test Image][image2]

Here is the undistorted version of this test image:
![Undistorted Test Image][image3]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps at lines # through # in `another_file.py`).  Here's an example of my output for this step.  (note: this is not actually from one of the test images)

![Binary Image][image4]
![Binary Masked Image][image5]


#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warper()`, which appears in lines 1 through 8 in the file `example.py` (output_images/examples/example.py) (or, for example, in the 3rd code cell of the IPython notebook).  The `warper()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
dst_x_offset = 300
src = np.float32([[img_size_x/2 - 56, 456],[img_size_x/2 + 56, 456], \
				 [img_size_x/2 + 446, 719],[img_size_x/2 - 446, 719]])
dst = np.float32([[img_size_x/2 - dst_x_offset, 0],[img_size_x/2 + dst_x_offset, 0], \
				 [img_size_x/2 + dst_x_offset, 719],[img_size_x/2 - dst_x_offset, 719]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 584, 456      | 340, 0        | 
| 696, 456      | 940, 0	    |
| 1086, 719     | 940, 719      |
| 194, 719      | 340, 719      |


![alt text][image6]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image7]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image8]


#### 7. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./output_videos/project_video_out.mp4).
And [here you can find](./output_videos/challenge_video_out.mp4) the output of my pipeline for the challenge video.


### IV. The classes Line and Lane

The classes `Line` and `Lane` keep track of all the interesting parameters that the pipeline detects from frame to frame. See code cell no. 5 in the Jupyter notebook.

#### 1. The Line class

#### 2. The Lane class


### V. Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
