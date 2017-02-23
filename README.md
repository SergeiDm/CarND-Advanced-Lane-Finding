# CarND-Advanced-Lane-Finding
## Project Description
The goal of the project is to detect lane lines on a road by processing video. The process of the video includes the following steps:
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
In 'Camera Сalibration' section of 'Advanced_Lane_Finding_Solution.ipynb' script we can see results of defining internal corners. Here is an example:
![Camera_calibration](https://github.com/SergeiDm/CarND-Advanced-Lane-Finding/blob/master/output_images/Camera_calibration.png)
For some calibration images in folder 'camera_calibration', the function 'cv2.findChessboardCorners' didn't find corners, because part of chessboard displayed on these images doesn't include all 9x6 corners which we are looking for.

## Pipeline (single images)
### Distortion Correction
Distortion correction of test images was provided by matrix and distortion coefficients calculted in previous step:
![Distortion_correction](https://github.com/SergeiDm/CarND-Advanced-Lane-Finding/blob/master/output_images/Distortion_correction.png)
If take a look at a car (the closest to the right border of images), we can see the different distances between the car and right border.
Other undistorted images can be found in 'output_images' folder (filenames have prefix 'undist').
