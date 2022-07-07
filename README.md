# Whereby Android SDK
[Whereby](https://whereby.com/) is the easiest way to embed video calls into your web page or mobile app.

This repository contains the Whereby framework for Android platform. You will find complete running examples using the SDK, in both Java and Kotlin, in our [demo apps](https://github.com/whereby/android-sdk-demo-app).

Other platforms: 
- [iOS SDK](https://github.com/whereby/ios-sdk) with sample app in Swift
- [Browser SDK](https://github.com/whereby/browser-sdk)

*Be informed that this is a beta version of the framework. For any comments, suggestions or questions, please reach out to our customer support: embedded@whereby.com* 

## Prerequisites
- [Signup](https://whereby.com/org/signup/embedded) for a Whereby Embedded account
- [Create a room](https://docs.whereby.com/creating-and-deleting-rooms) and get the roomURL
- Have [Android Studio](https://developer.android.com/studio/install) installed with a [running Android app](https://developer.android.com/studio/run)

## Installation 
The framework is distributed using [Jitpack](https://docs.jitpack.io/). If it is not already the case, add the Maven and Jitpack repositories to your Android project. This can be done either in the project  **build.gradle** file (not to be confused with the *module* build.gradle file) or in the **settings.gradle** file:
```
repositories {
    ...
    mavenCentral()
    maven { url 'https://jitpack.io' }  
 }
```

Then, check the [most recent version released](https://github.com/whereby/android-sdk/releases) and fetch the framework in the module **build.gradle** file:
```
dependencies {  
    ...
    def WHEREBY_SDK_VERSION = 'X.X.X'
    implementation("com.github.whereby:android-sdk:$WHEREBY_SDK_VERSION@aar") { transitive = true }
}

```
*Remember to sync your project after updating the gradle files.*

## Android minimum sdk version
In the *module* **build.gradle** file, make sure the minimum API level is equal or greater than **23**: 
```
android {  
    ...
    defaultConfig {  
        ...
        minSdk 23
    } 
}
```
*Remember to sync your project after updating the gradle files.*

## Usage
1. Setup your `WherebyRoomFragment` object in your source file and layout:
```
WherebyRoomFragment roomFragment = new WherebyRoomFragment();
```
```
<FrameLayout  
  android:id="@+id/layout_whereby_fragment_container"  
  .../>
  ```

2. Provide the room URL
```
URL roomURL = new URL("https://...");
```

3. Create a *WherebyRoomConfig* to be passed to the fragment
```
WherebyRoomConfig roomConfig = new WherebyRoomConfig(roomURL);
```

Optionally, you can customize the room:
```
roomConfig.setCameraEnabledAtStart(true);  
roomConfig.setMicrophoneEnabledAtStart(true); 
roomConfig.setDisplayName("Participant name");  
// ...
```

4. Setup an asynchronous event listener (optional)

This allows to receive events during the meeting through a list of callbacks methods:
```
roomFragment.setEventListener(new WherebyEventListener() {  

    @Override  
    public void onMicrophoneToggled(boolean enabled) {  
        // Update custom microphone button, for instance
    }  
  
    @Override  
    public void onCameraToggled(boolean enabled) {  
        // Update custom camera button, for instance
    }  

    // ...
});  
```

5. Send commands once the meeting has started (optional)
```
toggleCameraButton.setOnClickListener(view -> roomFragment.toggleCameraEnabled());  
toggleMicrophoneButton.setOnClickListener(view -> roomFragment.toggleMicrophoneEnabled());
```

6. Load the fragment with its arguments and join the meeting
```
Bundle bundle = new Bundle();  
bundle.putSerializable(ROOM_CONFIG_KEY, roomconfig);  
roomFragment.setArguments(bundle);  
FragmentManager fragmentManager = getSupportFragmentManager();  
FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();  
fragmentTransaction.replace(frameLayout, roomFragment);  
fragmentTransaction.commit();  
roomFragment.join();
```

7. Remove the fragment after the meeting has ended (optional)
```
FragmentManager fragmentManager = getSupportFragmentManager();  
FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();  
fragmentTransaction.remove(mRoomFragment);  
fragmentTransaction.commit();  
mRoomFragment = null;
```



