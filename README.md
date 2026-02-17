# Android SDK for Whereby Embedded
[Whereby](https://whereby.com/) is the easiest way to embed video calls into your web page or mobile app.

This repository contains the Whereby framework for Android platform. You will find complete running examples using the SDK, in both Java and Kotlin languages, in our [demo apps](https://github.com/whereby/android-sdk-demo-app).

Other platforms: 
- [iOS SDK](https://github.com/whereby/ios-sdk)
- [Browser SDK](https://github.com/whereby/browser-sdk)

*For any comments, suggestions or questions, please reach out to our customer support: embedded@whereby.com* 

## Prerequisites
- [Signup](https://whereby.com/org/signup/embedded) for a Whereby Embedded account
- [Create a room](https://docs.whereby.com/creating-and-deleting-rooms) and get the roomURL
- Have [Android Studio](https://developer.android.com/studio/install) installed with a [running Android app](https://developer.android.com/studio/run)

## Installation 
The framework is distributed using [Jitpack](https://docs.jitpack.io/). If it is not already the case, add the Maven and Jitpack repositories to your Android project. This can be done either in the project  **build.gradle** file (not to be confused with the *module* build.gradle file) or in the **settings.gradle** file:
```gradle
repositories {
    ...
    mavenCentral()
    maven { url 'https://jitpack.io' }  
 }
```

Then, add the Whereby SDK dependency to your module **build.gradle** file. You can specify either a full version name `X.X.X` (see the [list of released versions](https://github.com/whereby/android-sdk/releases)) or simply use a range format `X.+`:
```gradle
dependencies {  
    ...
    def WHEREBY_SDK_VERSION = '1.+'
    implementation("com.github.whereby:android-sdk:$WHEREBY_SDK_VERSION@aar") { transitive = true }
}
```
*Remember to sync your project after updating the gradle files.*

## Android minimum sdk version
In the *module* **build.gradle** file, make sure the minimum API level is equal or greater than **23**: 
```gradle
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
1. Import `WherebySDK` at the top of your file (typically this would be in your `Activity` or `Fragment` subclass):
    ```java
    import com.whereby.sdk.*;
    ```

2. Provide the room URL. The room URL would usually be created by your own backend using Whereby API (for more details see [Creating and deleting rooms](https://docs.whereby.com/creating-and-deleting-rooms)):
    ```java
    // Replace with your own:
    String urlString = "https://whereby-room-url";
    ```

3. Create a *WherebyRoomConfig*:
    ```java
    WherebyRoomConfig config = new WherebyRoomConfig(urlString);
    ```

    Optionally, you can customize the room:
    ```java
    config.setDisplayName("Participant name");
    config.setRoomBackgroundVisible(false); 
    // ...
    ```
4. Setup your `WherebyRoomFragment` in your source file (and layout):
    ```java
    WherebyRoomFragment fragment = WherebyRoomFragment.newInstance(config);
    ```
     
    Optionally, you can setup an asynchronous event listener. This allows to receive events during the meeting through a list of callbacks methods:
    ```java
    fragment.setListener(new WherebyRoomListener() {
    
        @Override  
        public void onMicrophoneToggled(boolean enabled) {  
            // Update custom microphone button
        }  
      
        @Override  
        public void onCameraToggled(boolean enabled) {  
            // Update custom camera button
        }  
    
        // ...
    });  
    ```

    You can also create your own buttons to send commands once the meeting has started:
    ```java
    toggleCameraButton.setOnClickListener(view -> fragment.toggleCameraEnabled());  
    toggleMicrophoneButton.setOnClickListener(view -> fragment.toggleMicrophoneEnabled());
    ```

5. Load the fragment to join the meeting:
    ```java
    getSupportFragmentManager()
      .beginTransaction()
      .replace(layout_whereby_fragment_container, fragment, TAG_ROOM_FRAGMENT)
      .commit();
    ```

    Then, the fragment can be removed after the meeting has ended:
    ```java
    FragmentManager fm = getSupportFragmentManager();
    Fragment existing = fm.findFragmentByTag(TAG_ROOM_FRAGMENT);
    fm.beginTransaction()
      .remove(existing)
      .commit();
    ```

6. We recommend to add the following `androidConfig` in the `AndroidManifest`, to avoid the activity to be destroyed and re-created on screen rotation:
    ```xml
    <application
        ...
        <activity
            ...
            android:configChanges="orientation|screenSize">
        </activity>
    </application>
    ```
    
## Disclaimer
Whereby publishes these packages to help the developer community understand how the Whereby Embedded product can be implemented.

Whereby does not recommend using such examples in a production environment without a prior assessment and appropriate testing relevant to the production setup targeted which can be of operational, technical, security or legal (e.g. library licenses assessment) nature. You expressly agree that the use of these packages is at your sole risk.

Whereby, its affiliates, suppliers, or licensors, whether express or implied, do not make any representation, warranties, contractual commitments, conditions, or assurances by the operation of these examples, or the information, content, materials, therein.

