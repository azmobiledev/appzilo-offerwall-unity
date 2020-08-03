## Appzilo Offerwall Unity

Our Plugin allows you to launch the Appzilo Offerwall as an activity on top of your application. Appzilo Unity Android Plugin is provided via Github containing unity_az_offerwall_sdk.unitypackage file. We currently only support for Android devices but iOS will be implemented in near future.

Appzilo offerwall for Android support API 17 and above.


## Installation

Import unity_az_offerwall_sdk.unitypackage into your project.

From Unity go to Assets menu → Import package → Custom package → choose our unity package.

## Usage Explanation

Import sdk
```
const string pluginName = "com.appzilo.sdk.Offerwall";

pluginClass = new AndroidJavaClass(pluginName);
AndroidJavaClass UnityPlayer;
UnityPlayer = new AndroidJavaClass("com.unity3d.player.UnityPlayer");
currentActivity = UnityPlayer.GetStatic<AndroidJavaObject>("currentActivity");
```

Initialize and open Offerwall
```
Dictionary<string, string> parameters = new Dictionary<string, string>();
AndroidJavaObject hashmap = ConvertDictionaryToJavaMap(parameters);
pluginClass.CallStatic("initApp", currentActivity, APP_KEY, USER_ID, hashmap);
pluginClass.CallStatic("show");
```

Checking callback (if enable screenshot), click on screenshot notification will redirect back to offerwall page
```
void OnApplicationPause(bool pauseStatus)
{
    if (!pauseStatus) {
        //after screenshot, redirect to open offerwall
        AndroidJavaObject intent = currentActivity.Call<AndroidJavaObject>("getIntent");
        pluginContext = currentActivity.Call<AndroidJavaObject>("getApplicationContext");
        pluginClass.CallStatic("onNewIntent", pluginContext, intent);
    }
}
```
Please refer to AZOfferwallPluginManager.cs to review the full code.

-------------------------------

# Permissions

We include the [INTERNET](http://developer.android.com/reference/android/Manifest.permission.html#INTERNET) and [ACCESS_NETWORK_STATE](https://developer.android.com/reference/android/Manifest.permission.html#ACCESS_NETWORK_STATE) permissions by default as we need it to make network requests and check if the network is available:

```
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```  

We also include the [FOREGROUND SERVICE](https://developer.android.com/guide/components/services#Foreground) permission in our latest sdk to support mediaprojection that targetsdkversion to 29
```
<uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>
```

The permissions [WRITE_EXTERNAL_STORAGE](https://developer.android.com/reference/android/Manifest.permission#WRITE_EXTERNAL_STORAGE), [SYSTEM_ALERT_WINDOW](https://developer.android.com/reference/android/Manifest.permission#SYSTEM_ALERT_WINDOW) and 
[PACKAGE_USAGE_STATS](https://developer.android.com/reference/android/Manifest.permission#PACKAGE_USAGE_STATS) are needed to you to add it manually in order to enable the screenshot function, which will help reduce potential spam and increase submission success rate

```
// mandatory to unlock more screenshot tasks
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

```
//Optional
<uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />
<uses-permission android:name="android.permission.PACKAGE_USAGE_STATS" tools:ignore="ProtectedPermissions" />

```
In order to do so, please go to File > Build Settings > Android > Player Settings > Publishing Settings > `Tick` Custom Main Manifest.
AndroidManifest.xml will be appeared inside Project_path/Plugins/Android/. Adding above permissions will unlock more tasks that needed for screenshot. 




