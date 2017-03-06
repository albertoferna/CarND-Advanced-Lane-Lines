# List of Output Images
Images in this folder are taken from the test_image folder.

- Undistorted images. These images are just the original with undistorted applied. The file name has the form “original_file_name”_undistorted. As an example, calibration1.jpg becomes calibration1_undistorted.jpg

- Color Filter. Image test1 is transformed to test_color_filter_channel_R by thresholding the red channel. tes1_color_filter_channel_S is a result of the same operation (with different parameters) applied on channel S. Values used in the filter can be seen in the notebook “Advanced Lane Finding”. Image test1_color_filter_mixed_thresholding_binary is the result of combining both color thresholding filtering and converting to binary.

- Gradient Filter. Gradient filtering is also applied to image test1 too. There is an instance of direction thresholding and an instance of magnitude thresholding. They are identified as test1_gradient_direction/magnitude. The combination of both thresholds and conversion to binary is presented in test1_gradient_thresholded_binary

- Thresholded images. This images are the result of applying thresholding combination to the images in test_folder. They have the same name followed by thresholded. As an example, straight_lines1.jpg becomes straight_line1_thresholded.jpg. The thresholding process followed is explained in the notebook “Advanced Lane Finding”

- Warped images. Image test4 is used as an example of warping an undistorted, thresholded image. It thus becomes test4_undistorted_thresholded_warped. straight_lines1_undistorted_warped is a version of straight_lin	es1 after applying undistort and warp.

- Result images. Images with a name ending in result are consequence of applying all the steps needed in the pipeline to static images
