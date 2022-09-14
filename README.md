# flam-cam-android-sdk

[![](https://jitpack.io/v/homingos/flam-cam-android-sdk.svg)](https://jitpack.io/#homingos/flam-cam-android-sdk)

```java
<activity
    android:name="com.flam.flamcam.FlamCamActivity"
    android:screenOrientation="fullSensor"
    android:configChanges="mcc|mnc|locale|touchscreen|keyboard|keyboardHidden|navigation|orientation|screenLayout|uiMode|screenSize|smallestScreenSize|fontScale|layoutDirection|density"
    android:hardwareAccelerated="false"
    android:process=":Unity">
</activity>
```

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
