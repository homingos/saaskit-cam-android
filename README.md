# Flam Cam SaaS Android SDK
Client Side Android SDK for Flam Cam SaaS.
If you want to read the full API documentation of SaaS SDK, see [here].

## Index

- [Installation](#installation)
- [Sample](#sample)
- [IssueReporting](#issuereporting)
- [Author](#author)

## Installation

- Download and place the [flam-cam.aar] into your project libs folder.
- Open settings.gradle file and add the following in pluginManagement and dependencyResolutionManagement repositories block
- Or open app level build.gradle(Module: *.app) file and add the following in allprojects block
```
flatDir {
    dirs 'libs'
}
```
- Open app level build.gradle(Module: *.app) file and add the following in dependencies block
```
dependencies {
    implementation(name: 'flam-cam', ext: 'aar')
}
```
- Add the following lines in gradle.properties file in the application root folder
```
org.gradle.jvmargs=-Xmx4096M
org.gradle.parallel=true
unityStreamingAssets=
unityTemplateVersion=3
```
- Click Sync Now to do a project sync since Gradle files have been modified
- Add the FlamCamActivity to your app AndroidManifest.xml application tag as shown below
```java
<activity
    android:name="com.flam.flamcam.FlamCamActivity"
    android:screenOrientation="fullSensor"
    android:configChanges="mcc|mnc|locale|touchscreen|keyboard|keyboardHidden|navigation|orientation|screenLayout|uiMode|screenSize|smallestScreenSize|fontScale|layoutDirection|density"
    android:hardwareAccelerated="false"
    android:process=":Unity">
</activity>
```
- Use below mentioned methods to load, unload Flam Cam.\
To load the Flam Cam use the below parameters, All parameters are required unless otherwise stated.

| Option        | Type              | Description                                                         |
| :------------ | ----------------- | ------------------------------------------------------------------- |
| `clientKey`   | string (required) | Flam SDK key recieved from `business.flamapp.com` after signin up.  |
| `privateKey`  | string (required) | Flam SDK private key recieved from `business.flamapp.com` after signin up. |
| `clientName`  | string (required) | Flam SDK name recieved from `business.flamapp.com` after signin up. |
| `clientSource`| string (required) | SAAS                                                                |

```java
public void loadFlamCam() {
    //Setting the boolean false
    isFlamCamLoaded = true;
    //Create the intent of Flam Cam and Start Activity
    Intent intent = new Intent(this, FlamCamActivity.class);
    intent.setFlags(Intent.FLAG_ACTIVITY_REORDER_TO_FRONT);
    intent.putExtra("clientKey", "EnterSaaSKeyHere");
    intent.putExtra("clientName", "EnterSaaSNameHere");
    intent.putExtra("clientSource", "SAAS");
    startActivityForResult(intent, 1);
}

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    if (requestCode == 1) isFlamCamLoaded = false;
}

public void unloadFlamCam() {
    if (isFlamCamLoaded) {
        //Use this method to close the camera
        flamBackButtonCallback.closeFlamcam();
        //or you can also use the below method
        //FlamCamActivity.instance.finish();
        //Setting the boolean false
        isFlamCamLoaded = false;
    }
}

@Override
public void onBackPressed() {
    if (isFlamCamLoaded) {
        //Use this method to put flamcam in background and load your activity in foreground
        flamBackButtonCallback.putFlamcamInBackground(this);
    } else {
        finishAffinity();
    }
}
```

## Sample

Below is the sample activity code

```java
package com.flamcam.sampleapp;

import android.content.Intent;
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
import androidx.appcompat.widget.Toolbar;
import android.view.View;
import android.widget.Toast;

import com.flam.flamcam.FlamCamActivity;
import com.flam.flamcam.FlamBackButtonCallback;

public class MainActivity extends AppCompatActivity {

    boolean isFlamCamLoaded = false;
    FlamBackButtonCallback flamBackButtonCallback = new FlamBackButtonCallback();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Toolbar toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
    }

    @Override
    protected void onNewIntent(Intent intent) {
        super.onNewIntent(intent);
        setIntent(intent);
    }

    public void loadFlamCam() {
        //Setting the boolean false
        isFlamCamLoaded = true;
        //Create the intent of Flam Cam and Start Activity
        Intent intent = new Intent(this, FlamCamActivity.class);
        intent.setFlags(Intent.FLAG_ACTIVITY_REORDER_TO_FRONT);
        intent.putExtra("clientKey", "EnterSaaSKeyHere");
        intent.putExtra("privateKey", "EnterPrivateKeyHere");
        intent.putExtra("clientName", "EnterSaaSNameHere");
        intent.putExtra("clientSource", "SAAS");
        startActivityForResult(intent, 1);
    }

    public void btnLoadFlamcam(View v) {
        loadFlamCam();
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        if (requestCode == 1) isFlamCamLoaded = false;
    }

    public void unloadFlamCam(Boolean doShowToast) {
        if (isFlamCamLoaded) {
            //Use this method to close the camera
            flamBackButtonCallback.closeFlamcam();
            //or you can also use the below method
            //FlamCamActivity.instance.finish();
            //Setting the boolean false
            isFlamCamLoaded = false;
        } else if (doShowToast) {
            showToast("Start Flamcam First");
        }
    }

    public void btnUnloadFlamcam(View v) {
        unloadFlamCam(true);
    }

    public void showToast(String message) {
        CharSequence text = message;
        int duration = Toast.LENGTH_SHORT;
        Toast toast = Toast.makeText(getApplicationContext(), text, duration);
        toast.show();
    }

    @Override
    public void onBackPressed() {
        if (isFlamCamLoaded) {
            //Use this method to put flamcam in background and load your activity in foreground
            flamBackButtonCallback.putFlamcamInBackground(this);
        } else {
            finishAffinity();
        }
    }
}
```

## IssueReporting

If you have found a bug or if you have a feature request, please report them at this [Support Center].

## Author

[Flam](<[https://flamapp.com/](https://flamapp.com/)>)

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

[here]: <https://business.flamapp.com>
[flam-cam.aar]: <https://github.com/homingos/flam-cam-android-sdk/blob/main/flam-cam.aar>
[Support Center]: <https://help.flamapp.com>
[Flam]: <https://flamapp.com>
