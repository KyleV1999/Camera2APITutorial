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
| CaptureRequest.Builder               | Obtain builder instance. Initialize request fields to a template in CameraDevice.                       |
| TotalCaptureResult                   | Final image captured from camera.                                                                       |

## Methods & Attributes

These are the important methods that will be used to implement the camrea, display it on a Texture view and prepare the image for saving to external storage.

  **Texture View Definition**: The texture view is used to display a content stream such as a video or OpenGL sene.


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
| build()                                                                                                    | CaptureRequest.Builder               | CaptureRequest          | Build a capture request using the current target surfaces and settings.                     |
| addTarget (Surface outputTarget)                                                                           | CaptureRequest.Builder               | void                    | Create add surface to list.                                                                 |
| set (Key <T> key, T value)                                                                                 | CaptureRequest                       | <T> void                | Set a capture request field to a value.                                                     |
  
 ## Example Code
 
To begin you must remember to give your app premissions to access your device's camera and the ability to write to the external storage in the Android Manifest. You also need to add ```android.hardware.camera2.full``` as a uses-feature to filter your app from devices that do not meet its hardware requirements.

```XML
    <uses-permission android:name="android.permission.CAMERA"></uses-permission>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"></uses-permission>
    <uses-feature android:name="android.hardware.camera2.full"></uses-feature>
```

Create a new TextureView.SurfaceTextureListener. When the TextureView's  SurfaceTexture is ready for use call a function to open the camera.

```java
 //Create new TextureView.SurfaceTextureListener.
    TextureView.SurfaceTextureListener textureListener = new TextureView.SurfaceTextureListener() {

     //Invoked when a TextureView's SurfaceTexture is ready for use.

        @Override
        public void onSurfaceTextureAvailable(SurfaceTexture surfaceTexture, int i, int i1) {

            try {
                openCamera(); //Call open camera function.
            } catch (CameraAccessException e) {
                e.printStackTrace();
            }

        }
```
Funtion to open camera. Create a CameraManager variable that will connect to the camera and store its information for use. You will also need to get the cameraId for the camera device. With the camera id get the camera's characteristics. Next store the characteristics in StreamConfigurationMap to set up Surfaces for creating a capture session.  Finally open the camera with the openCamera function.

```java
    private void openCamera() throws CameraAccessException {

        //Connect to system camera.
        CameraManager manager = (CameraManager) getSystemService(Context.CAMERA_SERVICE);

        cameraId = manager.getCameraIdList()[0]; //0 = rear camera.

        CameraCharacteristics characteristics = manager.getCameraCharacteristics((cameraId));

        StreamConfigurationMap map = characteristics.get(CameraCharacteristics.SCALER_STREAM_CONFIGURATION_MAP);

        imgDim = map.getOutputSizes(SurfaceTexture.class)[0]; //get image dimensions.

        //Check if required permissions are available.
        if(ActivityCompat.checkSelfPermission(this, Manifest.permission.CAMERA) != PackageManager.PERMISSION_GRANTED
        && ActivityCompat.checkSelfPermission(this,Manifest.permission.WRITE_EXTERNAL_STORAGE)!= PackageManager.PERMISSION_GRANTED){
            ActivityCompat.requestPermissions(MainActivity.this,new String[]{Manifest.permission.CAMERA,
                    Manifest.permission.WRITE_EXTERNAL_STORAGE},101);
            return;
        }

        manager.openCamera(cameraId,stateCallback,null); //Open a connection to a camera with the given ID.

    }
 ```
 
 Check the camera's state with ```CameraDevice.StateCallback``` object. When the camera is opened create the camera preview.
 ``` java 
     private final CameraDevice.StateCallback stateCallback = new CameraDevice.StateCallback() {
        @Override
        public void onOpened(@NonNull CameraDevice camera) {
            cameraDevice = camera;
            try {
                createCameraPreview(); //When the camera is opened start the preview.
            } catch (CameraAccessException e) {
                e.printStackTrace();
            }

        }
  ```
This is the function to create the camera preview. First you need to display the preview on the texture view. After that create a CaptureRequest.Builder for new capture requests, initialized with template for a target use case. Next create a new camera capture session by providing the target output set of Surfaces to the camera device. When the camera is doen configureing the session can start processing capture requests.

```java
    private void createCameraPreview() throws CameraAccessException {
        //Display preview on texture view.
        SurfaceTexture texture = textureView.getSurfaceTexture();
        texture.setDefaultBufferSize(imgDim.getWidth(),imgDim.getHeight());
        Surface surface = new Surface(texture);

        //Create a CaptureRequest.Builder for new capture requests, initialized with template for a target use case.
        CRB = cameraDevice.createCaptureRequest(CameraDevice.TEMPLATE_PREVIEW);
        CRB.addTarget(surface); //Add camera request builder to surface.
        //Create a new camera capture session by providing the target output set of Surfaces to the camera device.
        cameraDevice.createCaptureSession(Arrays.asList(surface), new CameraCaptureSession.StateCallback() {
            @Override
            //This method is called when the camera device has finished configuring itself, and the session can start processing capture requests.
            public void onConfigured(@NonNull CameraCaptureSession cameraCaptureSession) {
                if(cameraDevice == null){
                    return;
                }

                CCS = cameraCaptureSession;
                try {
                    updatePreview();
                } catch (CameraAccessException e) {
                    e.printStackTrace();
                }
            }

            @Override
            public void onConfigureFailed(@NonNull CameraCaptureSession cameraCaptureSession) {
                Toast.makeText(getApplicationContext(),"Configuration Changed",Toast.LENGTH_LONG).show();
            }
        },null);
    }
    
        private void updatePreview() throws CameraAccessException {
        if(cameraDevice == null){
            return;
        }

        CRB.set(CaptureRequest.CONTROL_MODE, CameraMetadata.CONTROL_MODE_AUTO);

        CCS.setRepeatingRequest(CRB.build(),null,mBackgroundHandler);
    }
    
  ```
  
 

 
 
 
 







 
  
  
 
