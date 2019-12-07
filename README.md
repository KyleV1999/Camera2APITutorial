# Camera2 Tutorial

## Introduction

This is a tutorial on how to develop your Android app with the Camera2 API in Android Studio. The ```android.hardware.camera2```package replaces the deprecated Camera class and provides an interface to individual camera devices connected  to an Android Device. It is used to build a camera view within your app without having to go to the actual camera app though an intent if you wanted to use the camera. The Camera2 API gives a lot of flexibility over the use of the device's camera. For example, the Camera2 app allows images to be taken at faster intervals, show previews from multiple cameras, apply effects and filters to the camera directly and supports capturing uncompressed images. The Camera2 API will only work on devices that support it.


## History
Camera2 was introduced to Android Lollipop (API 21). The Camera2 API was introduced to replace the original Camera API (Which was added in API 1). The original Camera API was slow and lacked many features such as capturing uncompress images. Camera 2 gives developers and photographers more control or the camera within the app with better performance. The features of camera2 can be used in your project by importing the ```android.hardware.camera2```package.

## Classes 

Here is a breakdown of some of the classes in the ```android.hardware.camera2```package needed to build a basic camera app.

| Class                                | Description                                                                                             |
|--------------------------------------|---------------------------------------------------------------------------------------------------------|
| CameraCaptureSession                 | A capture session configured for a CameraDevice. Used for capturing images from the camera.             |
| CameraCaptureSession.CaptureCallback | A callback object that tracks the progress of a CaptureRequest that was submitted to the camera device. |
| CameraDevice                         | Represents a single camera device connected to and Android Device                                       |
| CameraManager                        | Manages system services for detecting, defining and connecting to CameraDevice.                         |
| CameraMetaData                       | Controls camera and gets camera information.                                                            |
| CaptureRequest                       | Settings and outputs needed to capture and image from the camera device.                                |
| TotalCaptureResult                   | Final image captured from camera.                                                                       |

## Methods & Attributes

| Method                                                                                                     | Class                                | Return Values           | Description                                                                                 |
|------------------------------------------------------------------------------------------------------------|--------------------------------------|-------------------------|---------------------------------------------------------------------------------------------|
| capture (CaptureRequest request, CameraCaptureSession.CaptureCallbacklistener, Handler handler)            | CameraCaptureSession                 | abstract int            | Request for and image to be captured by camera device.                                      |
| close()                                                                                                    | CameraDevice                         | abstract void           | Close the connection to this camera device.                                                 |
| onCaptureCompleted (CameraCaptureSession session, CaptureRequest request, TotalCaptureResult result)       | CameraCaptureSession.CaptureCallback | void                    | Method called when image capture fully completed and result metadata is available.          |
| createCaptureRequest (int templateType)                                                                    | CameraDevice                         | CaptureRequest. Builder | Create a CaptureRequest.Builder for capture request.                                        |
| createCaptureSession (List<Surface> outputs,  CameraCaptureSession.StateCallback callback,Handler handler) | CameraDevice                         | abstract void           | Create camera capture session. Provide the target output set of Surfaces to camera device.  |
| getId()                                                                                                    | CameraDevice                         | abstract String         | Get id of camera device.                                                                    |
| openCamera (String cameraId, Executor executor, CameraDevice.StateCallback callback)                       | CameraManager                        | void                    | Open camera of given id.                                                                    |
| getCameraCharacteristics (String cameraId)                                                                 | CameraManager                        | CameraCharacteristics   | Get properties of camera device.                                                            |



