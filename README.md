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

Checking callback (if enable screenshot)
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


