# Advanced Lane Finding Project

## Camera Calibration

* The image must first be converted to a gray image.

* Defining the object points in 3d.

* Using `cv2.calibrateCamera` function in `openCV` to get the necessary values for calibration.

Using the values ``ret``, ``mtx`` and ``dist`` and the function ``cv2.undistort`` to get the undistorted image. The result is showed below.

<img src="https://filedn.com/lUE8ye7yWpzFOF1OFLVsPau/Github/laneLines_advanced/calibrationCamera.png" alt="camera calibration" width="888">

## Pipeline (single image)

### 1. Image Undistortion

The values ``ret``, ``mtx`` and ``dist`` are calculated from the section above. Using these values
and the function ``cv2.undistort`` to get the undistorted image. The result is showed below.

<img src="https://filedn.com/lUE8ye7yWpzFOF1OFLVsPau/Github/laneLines_advanced/calibration.png" alt="undistort" width="888">

### 2. Image Processing

The image was first converted into `HLS` format and in `L` and `S` channels appropriate thresholdeds should be defined to extract infromations from the lines. The result is below.

<img src="https://filedn.com/lUE8ye7yWpzFOF1OFLVsPau/Github/laneLines_advanced/imgProcessing.png" alt="img processing" width="888">

### 3. Image Warping

The vertices as destination were simply defined as the four vertices in the corners of the image. The image was then warped from perspective view to birds-eye view. The Result is below.

<img src="https://filedn.com/lUE8ye7yWpzFOF1OFLVsPau/Github/laneLines_advanced/imgWarping.png" alt="img warping" width="888">

### 4. Lines Finding through Windows

	(1) From the bottom several lines of the picture, it would be calculated, at which two positions the white points were the most. These two positions would be considered as start positions of the two polynomials.

	(2) From the bottom up, small windows would be generated to detect where the most white points were.
	
	(3) All the points captured by the windows were collected. And through those points two polynomials were generated.

<img src="https://filedn.com/lUE8ye7yWpzFOF1OFLVsPau/Github/laneLines_advanced/windows.png" alt="windows" width="888">

### 5. Lines Finding through Previous Valid Polynomials

	(1) According to previous polynomials two sections (through offsets of the polynomials) were generated.

	(2) Inside these two sections the white points were detected.

	(3) Through those captured points two new polynomials were generated.

<img src="https://filedn.com/lUE8ye7yWpzFOF1OFLVsPau/Github/laneLines_advanced/poly.png" alt="poly" width="888">

### 6. Radius of the Curvature and Offset Calculation

* The radius of the curvature was calculated approximately according to the detected lines (polynomials).

<img src="https://filedn.com/lUE8ye7yWpzFOF1OFLVsPau/Github/laneLines_advanced/radiusCurvature.png" alt="radius curvature" width="388">

* It was assumed that the middle of the picture was the center of the vehicle, so that the offset of the vehicle could be calculated approximately according to the detected polymonials.

### 7. Images Merging

To notice that, the warped image muss firstly converted to perspective image then the two images can be merged.

<img src="https://filedn.com/lUE8ye7yWpzFOF1OFLVsPau/Github/laneLines_advanced/backWarping.png" alt="merge" width="888">

## Pipeline (video)

### 1. I definded a class called ``Line`` to store the information about the polynomials and do some changes to the stored data.

The class ``Line`` has the main functions below.

* ``calc_error`` calculate the error to test, if two lines are accepted to be parallel.

* ``smooth_poly`` update the values polynomials of this line(object) in comparision with another line(object). The new polynomials will be calculated as below.

```
poly_updated = (poly_previous + poly_current) * 0.5
``` 

This function can smooth the lanes detection between frames.

### 2. I defined a class called ``Organizer`` to control the whole video processing.
The ``organizer`` has a function ``process_img``. This function control the every image processing. When the ``organizer`` find that the detected lines are not accepted it will take the polynomial from the previous valid frame. The previous valid polynomials is saved in the ``organizer``.

A test result is below.

<img src="https://filedn.com/lUE8ye7yWpzFOF1OFLVsPau/Github/laneLines_advanced/prepVideo.png" alt="test img" width="888">

## Result and Final Effect

The final video is uploaded in Youtube [[link]](https://www.youtube.com/watch?v=H1SiaglfF5M&list=PLNDTbGbATLcED0iX8K-zY3vrNbwhxV8gC&index=1). 

A screenshot is below.

<img src="https://img.youtube.com/vi/H1SiaglfF5M/0.jpg" alt="screenshot" width="488">

## Problems Discussion

### 1. Parameter Tuning

* The thresholds for the image processing to detect important points are important to be tuned, so that the algorithm will be robuster.
A robust image processing algorithm need to be found because the algorithm above works well with little noise. This meas, that it works not very well for example, when the lane has strong shadows.

* The choise of the vanish point is important, bacause it decides whether the lines will be parallel in the birds-eye view.

### 2. Sequence of the Processing is also important

For example, the image should be converted to binary image before it is warped to birds-eye view, because in the birds-eye view the pixels above will be blurred, 
so that the image processing works worse than that the unwarped image will be processed.

## References

1. Udacity Self Driving Car Engineer Nanodegree.

2. [Radius of curvature (Wikipedia)](https://en.wikipedia.org/wiki/Radius_of_curvature)
