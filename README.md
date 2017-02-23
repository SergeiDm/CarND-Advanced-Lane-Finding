# CarND-Advanced-Lane-Finding
## Project Description
The goal of the project is to detect lane lines on a road, their curvature by processing video. The process of the video includes the following steps:
- Camera Calibration.
- Distortion Correction.
- Creating binary images.
- Perspective transform.
- Detection Lane Lines pixels.
- Determining the curvature of the lane and vehicle position.

## Project files
The project includes the following folder/files:
- camera_calibration – the folder with Chess Board images for a camera calibration.
- test_images - the folder with image examples for testing pipeline.
- output_images - the folder with results of testing all steps of pipeline.
- Advanced_Lane_Finding_Solution.ipynb - the script with pipeline.
- challenge_video.mp4 - the video to test pipeline.
- output_challenge_video_1.mp4 - the result of pipeline work on 'challenge_video.mp4'.
- project_video.mp4 - the video to test pipeline.
The result of pipeline work on 'project_video.mp4' is https://youtu.be/y2oDu1P6vjw.

## Camera Calibration
The images of chessboard (9 x 6 internal corners ) were taken for calibrating camera (see folder 'camera_calibration'). Chessboard image is a high contrast pattern which allows to easily detect internal cornenrs. The main idea consists of matching 3D real world points and its corresponding 2D image points. By using 'cv2.findChessboardCorners' function, internal corners were found and passed to 'cv2.calibrateCamera' function, which calculates the camera matrix and distortion coefficients.
In 'Camera Сalibration' section in 'Advanced_Lane_Finding_Solution.ipynb' script we can see results of defining internal corners. Here is an example:
![Camera_calibration](https://github.com/SergeiDm/CarND-Advanced-Lane-Finding/blob/master/output_images/Camera_calibration.png)
For some calibration images in folder 'camera_calibration', the function 'cv2.findChessboardCorners' didn't find corners, because part of chessboard displayed on these images doesn't include all 9x6 corners which we are looking for.

## Pipeline (single images)
### Distortion Correction
Distortion correction of test images was provided by matrix and distortion coefficients calculted in previous step:
![Distortion_correction](https://github.com/SergeiDm/CarND-Advanced-Lane-Finding/blob/master/output_images/Distortion_correction.png)
If take a look at a car (the closest to the right border of images), we can see the different distances between the car and right border.
Other undistorted images can be found in 'output_images' folder (filenames have prefix 'undist').
'Distortion Correction' step is in '1.1. Distortion Correction' section of 'Advanced_Lane_Finding_Solution.ipynb'.

### Creating binary images
Processing images we should decide which pixels are part of lane lines. In general lane lines tend to be vertical and have specific colors (yellow and white) which creates a contrast with road surface color. So, for finding lane lines we should reduce irrelevant information in an image by creating binary image. With a knowledge of verticality and colors of lane lines we can apply the following techniques:
- Color thresholding: I used V channels of YUV and HSV color spaces for thresholding ((0,110) for YUV, (240,255) for HSV). This allowed to clearly identify yellow color in different conditions: shadows, pavement color changes and so on.
- Gradient thresholding: I applied magnitude of gradients in both the x and y directions (Sobel operators), but there was used decreasing coefficient for y gradient, because of assumption 'lane lines tend to be vertical'.
Both color and gradient thresholding were combined by 'or' operator. 
Here's an example of this step:
![Binary_images](https://github.com/SergeiDm/CarND-Advanced-Lane-Finding/blob/master/output_images/Binary_images.png)
Other binary images can be found in 'output_images' folder (filenames have prefix 'binary').
'Creating binary images' step is in '1.2. Binary Image' section of 'Advanced_Lane_Finding_Solution.ipynb'.

### Perspective transform
It's complicated to define lane lines curvature with given image perspective, so it reasonable to 'convert' image points to a new perspective so called bird’s-eye view. In this case we will have view lane lines from above.
For this step there were chosen 4 source and 4 destination points. Source points are angular points of trapezoidal region, covering lane lines (see the left picture below). Destination points were chosen in such a way as to lane lines were parallel (see the left picture below):
![Perspective_transform](https://github.com/SergeiDm/CarND-Advanced-Lane-Finding/blob/master/output_images/Perspective_transform.png)
Here is result of applying perspective transform to binary images:
![Perspective_transform](https://github.com/SergeiDm/CarND-Advanced-Lane-Finding/blob/master/output_images/Perspective_transform2.png)
Lane lines look parallel, so we can identify their curvature.
Other images can be found in 'output_images' folder (filenames have prefix 'warped').
'Perspective transform' step is in '1.3. Perspective transform' section of 'Advanced_Lane_Finding_Solution.ipynb'.

### Detection Lane Lines pixels
For detecting lane lines after previous step I used 'Peaks in a Histogram' method. Firstly, there were calculated initial points for lane lines by using histogram (since our image has only '0' or '1' values). Secondly, 'sliding windows' were used finding and following lines.
For drawing found lane lines, I used second order polynomial to fit left and right line pixel (function 'process_image' in '1.4. Lane Lines Detection' section).
Here are results:
![Perspective_transform](https://github.com/SergeiDm/CarND-Advanced-Lane-Finding/blob/master/output_images/Lines_detection.png)
Other images can be found in 'output_images' folder (filenames have prefix 'prelim_result').

###









