# Camera2 Tutorial

## Introduction

This is a tutorial on how to develop your Android app with the Camera2 API in Android Studio. The ```android.hardware.camera2```package replaces the deprecated Camera class and provides an interface to individual camera devices connected  to an Android Device. It is used to build a camera view within your app without having to go to the actual camera app though an intent if you wanted to use the camera. The Camera2 API gives a lot of flexibility over the use of the device's camera. For example, the Camera2 app allows images to be taken at faster intervals, show previews from multiple cameras, apply effects and filters to the camera directly and supports capturing uncompressed images. The Camera2 API will only work on devices that support it.


## History
Camera2 was introduced to Android Lollipop (API 21). The Camera2 API was introduced to replace the original Camera API (Which was added in API 1). The original Camera API was slow and lacked many features such as capturing uncompress images. Camera 2 gives developers and photographers more control or the camera within the app with better performance. The features of camera2 can be used in your project by importing the ```android.hardware.camera2```package.

