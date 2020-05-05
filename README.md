Lane Detection Algorithm
==================

## Objective
The goal of this project is to develope an lane detection algorithm for LDW-systems(Lane Departure Warning Systems).

In this Project, we'll use a gradient-based method and a color-based method to extract information to identify road lanes.

## Files
>* [main.py](https://github.com/DuseobSong/Lane-Detection/blob/master/main.py): main code
>* [classes.py](https://github.com/DuseobSong/Lane-Detection/blob/master/classes.py): contains following classes
> > * class ***Calib***: to get and apply camera-matrix and distortion-coefficients
> > * class ***Mask***: filtering image with gradient-based method and corlor-space-based thresholding
> > * class ***Lane***: perspective transformation, find lane lines with sliding-window method
>* [functions.py](https://github.com/DuseobSong/Lane-Detection/blob/master/functions.py): functions to record results

## Developement Environment
>OS: Wndows 10 (x64), Python 3.5, OpenCV 4.2.0

## Structure
>![image](https://github.com/DuseobSong/Lane-Detection/blob/master/Flow_Charts/Structure.png)
>The main pipeline is devided into three main parts: camera calibration, image preprocessing and lane detection
>
>***1) Camera Calibration***- Some pinhole cameras introduce significant distortion to images with an erroneous infotmation. To Correct the distorted image, we need camera matrix and distortion coefficients, and they can be calculated using a set of chessboard images and applied to the video frames.
>   
>***2) Image Preprocessing*** - It's the most important part of this algotithm. First, video frames are transformed into YCrCb or HLS color space and separated into each channel. In this project, Y-, Cr- and S-channels are used. Using thresholding and gradient-based method we can identify plausible line-pixels.
>
>***3) Lane Detection*** - First, we apply perspective transform to preprocessed image to calculate the radius of curvature. We use a sliding-window method to identify the line pixels and to filter out the irrelevant pixels(such as cars or trees). And then lane lines are estimated from the detected line-pixels.
>
## Pipeline
>### 1. Camera calibration
> When a camera takes an image of 3D-object and transforms it into an 2D image, this image can be distorted and deliver erroneus informations.
> To correct the distortion, we must calculate a camera-matrix and distortion-coefficients with a set of chessboard images. In this project, [cv2.calibrateCamera()](https://docs.opencv.org/2.4/modules/calib3d/doc/camera_calibration_and_3d_reconstruction.html) function and a set of 20 chessboard images are used to compute these matrix and coefficients.
>
> The code for this process is contained in the ***Calib*** class in the [classes.py](https://github.com/DuseobSong/Lane-Detection/blob/master/classes.py).
>
>(Further information: [OpenCV Tutorial - Camera Calibration](https://docs.opencv.org/master/dc/dbb/tutorial_py_calibration.html))
>
> ![image](https://github.com/DuseobSong/Lane-Detection/blob/master/result/calibration/calibration.png)
>### 2. Image preprocessing
>![image](https://github.com/DuseobSong/Lane-Detection/blob/master/Flow_Charts/Preprocessing.png)
> #### ***2-1. Color space transform***
> 
> First, RGB-images are transformed into YCrCb- and HLS color spaces. In this project, we use Y-, Cr- and S-channel images to find lane lines. 
> - ***Y-channel***

> Y-channel images are similar to gray-scale images, so we can identify line pixels in these images using gradient approach. 
> ![image](https://github.com/DuseobSong/Lane-Detection/blob/master/result/img_preprocessing/cvtColor/Y.png)
> However, when the lane line is yellow and the road background color is light gray, it is difficult to distinguish the lane line from the background in gray-scale image. To solve this problem, we need Cr- or S-channel image.
> ![image](https://github.com/DuseobSong/Lane-Detection/blob/master/result/img_preprocessing/cvtColor/Y_prob.png)

> - ***S-channel*** 

> In General, since the lane lines has a high saturation value in clear weather, we can identify lane-line-pixels in S-channel images.
> ![image](https://github.com/DuseobSong/Lane-Detection/blob/master/result/img_preprocessing/cvtColor/S.png) 
> Note that shadows also have a high saturation value, so pixels with low values in the Y-channel image should be filtered out to identify relevant pixels.
> ![image](https://github.com/DuseobSong/Lane-Detection/blob/master/result/img_preprocessing/cvtColor/S_prob.png)

> - ***Cr-channel*** 

> In the Cr-channel image, the yellow line is easily detected, but the white lines are not identified. If the purpose is to detect only the white lines, you can use only Y- and S-channel images.
> ![image](https://github.com/DuseobSong/Lane-Detection/blob/master/result/img_preprocessing/cvtColor/Cr.png)

>#### ***2-2. Contrast enhancement***
![image](https://github.com/DuseobSong/Lane-Detection/blob/master/result/img_preprocessing/contrast_enhancement.png)

>#### ***2-3. Filtering with gradient information***
![image](https://github.com/DuseobSong/Lane-Detection/blob/master/result/img_preprocessing/gradient_filtering.png)

>#### ***2-4. Thresholding with color information***
![image](https://github.com/DuseobSong/Lane-Detection/blob/master/result/img_preprocessing/color_thresholding.png)

>#### Result

>### 3. Find lane
>![image](https://github.com/DuseobSong/Lane-Detection/blob/master/Flow_Charts/Detection.png)

## Result
>![image](https://github.com/DuseobSong/Lane-Detection/blob/master/result/gif/output.gif)
>
> ### 1. Bird-View image
>![image](https://github.com/DuseobSong/Lane-Detection/blob/master/result/gif/Bird_view.gif)
>
> ### 2. Camera image
>![image](https://github.com/DuseobSong/Lane-Detection/blob/master/result/gif/Camera_view.gif)
>
