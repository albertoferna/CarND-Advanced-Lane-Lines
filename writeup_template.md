## Writeup
**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.


### Camera Calibration

Camera calibration is done in cells 2 and 3 of the first notebook. It is pretty much straight out of what was seen in class. The main difference is that the number of corner points change because the board is 6 by 9. Also, corner detecting fails in some images and that has to be accounted for when we loop over them.

In the end, we find a camera matrix and distortion coefficients that I print and save as a numpy array for later use.

#### Provide an example of a distortion-corrected image.

I check that distortion correction is working by applying it to calibration1.jpg. This is a closeup of the board and it should be easy to see it the correction is working fine. This is done in cell 5. The result can be see in:
[image1]: ./output_images/calibration1_undistorted.png "Undistorted"

I also applied the same correction to one the test images as seen in cell 6.

### Thresholded binary image

To obtain a thresholded binary image I used both color transforms and gradient information. To get a sense of what information was on different channels, I plotted separately channels R, B, G, H, L, and S for image test1. I took that picture for testing as it is, together with test4 the hardest to get correctly from the basic video. This is done in cell 7.

#### Color Thresholding

Judging by the images, the most promising channels to use are red and saturation. I did several test on the thresholding values that seemed to work well for each channel. Given those thresholds, I filter any value outside that range while keeping values within. In this step I did not convert the image to a binary representation. This is done in cells 8 and 9. I took this approach because I wanted to combine the intensity values on each channel instead of doing a union or intersection of them. I wanted for each image to reinforce each other where lanes and detected. It seems to me a better way to make use of the information to apply a combined threshold afterwards as well as convert it to binary. To do that I had to normalize the sum of both images. That is done in cell 10.

#### Gradient Thresholding

I followed a very similar process with the gradients. I played for a while with threshold values to get a reasonable idea of what worked. After that, I applied the thresholding in the same way than for color, by filtering out values outside the thresholding range. This is done in cells 12 and 13.

I used the results from gradients as a sort of reinforcing mask. By combining the image obtained by color thresholding with both gradient images in a linear combination fashion we can make most of the non useful stuff disappear while maintaining the lane lines. To do that, I multiplied by some tested values each image and normalize the sum afterwards. Once that is done I applied a new threshold to the combination. This is done in cell 14.

This is one of the points that I think should require a more formal approach. I think that having a few images with the ground truth for this case and running a formal optimization over this parameters would help a lot in improving the detection capability.

After some trial an error testing I applied the procedure to all the testing images. Results can be seen as the output of cell 16







[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"

 
####3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warper()`, which appears in lines 1 through 8 in the file `example.py` (output_images/examples/example.py) (or, for example, in the 3rd code cell of the IPython notebook).  The `warper()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```
src = np.float32(
    [[(img_size[0] / 2) - 55, img_size[1] / 2 + 100],
    [((img_size[0] / 6) - 10), img_size[1]],
    [(img_size[0] * 5 / 6) + 60, img_size[1]],
    [(img_size[0] / 2 + 55), img_size[1] / 2 + 100]])
dst = np.float32(
    [[(img_size[0] / 4), 0],
    [(img_size[0] / 4), img_size[1]],
    [(img_size[0] * 3 / 4), img_size[1]],
    [(img_size[0] * 3 / 4), 0]])

```
This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 585, 460      | 320, 0        | 
| 203, 720      | 320, 720      |
| 1127, 720     | 960, 720      |
| 695, 460      | 960, 0        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

####4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image5]

####5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

####6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image6]

---

###Pipeline (video)

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

