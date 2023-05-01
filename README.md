# V_Snap
apply snap filters to static images

This SDK allows you to add Snapchat-like filters to your Android application. To use this SDK, you will need to add internet permission to your manifest file and follow the instructions below.

Installation
To install this SDK, add the following to your Manifest file:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

add meta data inside your application tag in manifest 

```xml
<application>
	<activity>
		.....
	<activity/>
	<meta-data
	    android:name="com.snap.camerakit.app.id"
	    android:value="YOUR_APP_ID" />
	<meta-data
	    android:name="com.snap.camerakit.api.token"
	    android:value="YOUR_API_TOKEN" /> 
<application/>
```
Import
download [VSnapModule.zip](https://github.com/lahsiv25/snap/blob/main/VSnapModule.zip "VSnapModule.zip") 
Extract the file and import VSnap as a module add the below line in build,gragle app dependency and sync the project
```xml
implementation project(path: ':V_Snap') 
```


Usage
To use this SDK, you will need to add the following code to your user activity/fragment xml:
```xml
<com.example.snaptest.VSnapView
    android:id="@+id/v_snap"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:lens_id="YOUR_LENS_ID"
    app:group_id="YOUR_GROUP_ID"
    app:is_fullscreen_filter="true"
    app:progress_time="10000"
    app:failure_time="20000"
    app:snap_gravity="BOTTOM_CENTER"
    app:preferred_output_type="BITMAP"/>
```
  
    
You will also need to add the following code to your activity or fragment:
```xml
val previewTextureView = findViewById<VSnapView>(R.id.v_snap)

previewTextureView.applySnapFilter(this, "YOUR_INPUT_IMAGE") { status ->
    when (status) {
        is FilterAppliedState.FAILED -> {
            "failed ${status.error.error} message ${status.error.message}".vlog()
        }

        FilterAppliedState.IN_PROGRESS -> {
            "in progress".vlog()
        }

        is FilterAppliedState.SUCCESS -> {
            "success output type ${status.outputType}".vlog()
            runOnUiThread {
                when(status.outputType){
                    SnapOutputType.BITMAP -> {
                        val output = status.result as Bitmap
                    }
                    SnapOutputType.FILE -> {
                        val output = status.result as File
                    }
                    SnapOutputType.URI -> {
                        val output = status.result as Uri
                    }
                    SnapOutputType.URI_STRING -> {
                        val output = status.result as String
                    }
                }
            }
        }
    }
}
```
 
  
  This code applies the filter to the input image and returns the result in the specified output type. If the filter fails to apply, the SDK returns an error message.
