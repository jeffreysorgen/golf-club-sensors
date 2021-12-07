AppDev.md

#
- Android Studio
- break out _completed_ section to its own page

[*[ top ]*](GolfSwingSensors.md/#golf-swing-sensors)
[*[ 1 ]*](implementation.md/#the-accelerometer)
[*[ 2 ]*](implementation.md/#solve-for-power)
[*[ 3 ]*](EnablingBLE.md#step-three)
**[_Next: [ 4 ]_](#step-four)**
[*[ Reference ]*](activity.md/#reference)
[*[ KWS ]*](KWS.md/#key-word-spotting)
[*[ 5 & 6 ]*](activity.md/#steps-five-and-six)
[_[ notes ]_](thoughtsandnotes.md/#other-projects)

##
##### Step Four:
# Enable Smartphone Response

**Description:**

Now that data is being transmitted from the Nano33BLESense, we need to have an Android application that receives it.

For development, we'll turn on and off the flashlight or get the phone to beep high and low, depending on the Resting state.
While this specific functionality won't be used in the end, the solution is crucial for when we're trying to get the phone to chirp good and bad golf swings, or take action in some other way.
- **Enable beeps in App before trying BLE**
    - Make buttons for flashlight (on, off, exit)
    - Convert function to beeps (high, low, exit) 

_I've already created the hello-world app a couple of times, and it's very simple and shows how to enable the app on the Android device.
There is another example app, a flashlight app, which will show how app functionality will work on the system. 
This is a good start to collecting various pieces that will get combined in the end. 
Hello World (kotlin, LinkedIn Learning), and then [Flashlight app](https://www.geeksforgeeks.org/how-to-build-a-simple-flashlight-torchlight-android-app/) or this other webpage with a [better more helpful video](https://programmerworld.co/android/how-to-switch-torch-or-flash-light-on-and-off-from-your-android-app-complete-source-code/)_

<-- -->

Go directly to beep.
WorkManager accesses the functions of this.
**Study WorkManager.**
For WorkManager to do something, it has to be triggered by something.
It needs to be enabled once there's a connection to BLE.
**So before WorkManager can do anything, the app needs to enable BLE.**
Working backwards, make the flashlight on/off with buttons. 

Truth is I'm going to have to build several example apps before I figure out how to do things on my own.
I installed the basic "hello world" app, but it didn't really teach me anything.
I still need to learn about the structure of application development.
It looks like there are a few basic files which are XML files.
There is also **Gradle** which is a build system.
Android Studio uses Gradle, which is a build automation tool.
There is Manifest, which seems like the starting point. _Is it?_
I'm trying to understand the flow of the code. 
I believe that Manifest file is where it starts.
Is it like Python where it goes line by line and runs stuff?

<-- -->

In terms of the Android application itself, first create the Hello World of Android development, and then the focus needs to be WorkManager functionality. This is where activity within the app interacts with other functions outside of the application.

**To do:**

1. Develop the Hello World of Android Apps (do this again)
1. Add buttons: "On" "Off" "Quit" (simple) There is a flashlight example app out there. _Find it._
1. Enable flashlight to turn on and off (use **WorkManager**)
    1. When on, "On" is greyed out, etc.
1. Then enable high and low tone beeps for each button
1. Add BLE capability (use WorkManager and do what it takes)
1. Move on to KWS

##### Development:

- Enable smartphone functions with Android Studio 
- Enable the app to act upon a BLE characteristic changing from 0 to 1

##
### Working with Android Studio

- Install Android Studio. (done)
- Buy _motion.golf_ domain. (done)
- **[Build first app.](https://developer.android.com/training/basics/firstapp)** (in progress)
  - **_WorkManager_** performs background tasks (so implement this for BLE connectivity?)
    - [WorkManager link to LinkedIn video](https://www.linkedin.com/learning/concurrent-programming-with-android-threads-workers-and-kotlin-coroutines/manage-deferrable-tasks-with-workmanager?autoAdvance=true&autoSkip=false&autoplay=true&resume=true)
    - "Do some work!" is the "hello world" of WorkManager (refer to video)
  - _Using minimum version API 19_ (default was 21) because of my old Android
- Enable GitHub connection:
  - Enable local authentication:
  - First challenge: `<img src="/images/gitloginlocal.PNG" width="50%">`
  - Solved with [this video: "Integrate Github Login with Android Studio"](https://www.youtube.com/watch?v=K0smcN8Q-XM)
  - Prompted within Android Studio that Git wasn't installed. Clicked "install" and it installed Git for Windows automatically.
  - Repository initialized locally, and once there's something there, we'll commit to GitHub. (in progress, learning)
  - Created 'my-first-app' repository via Android Studio!

Fixing stuff:
- Fixed GitHub stuff (above)
- Gradle license error: fixed by enabling Google Play Licensing Library
- Kotlin / Gradle version conflict: `Kotlin version that is used for building with Gradle (1.5.20) differs from the one bundled into the IDE plugin (1.5.31)` so I **updated** the build.gradle file to match (per LinkedIn course, "manage gradle build scripts" video)
- Avoid "+" character in the build.gradle file: `Avoid using + in version numbers; can lead to unpredictable and unrepeatable builds (junit:junit:4.+)` so I **updated** the "4.+" to current (4.13.2) (per [stackoverflow, here](https://stackoverflow.com/questions/46793888/build-gradle-warning-avoid-using-in-version-numbers)) and problem solved.
- At this point, once I found no problems in Studio, I did `file | invalidate caches and restart` to clean things up
- "Typo" name of package has been added to dict.
- IGNORING THIS: `On SDK version 23 and up, your app data will be automatically backed up and restored on app install. Consider adding the attribute 'android:fullBackupContent' to specify an '@xml' resource which configures which files to backup, or just set 'android:fullBackupOnly=true'. More info: https://developer.android.com/guide/topics/data/autobackup` and will come back if needed.
- Suppressed `jcenter()` (old repository) error because `mavenCentral()` repo is in the list already
- Set this from "true" to: `android:allowBackup="false"` so that the error goes. It was suggesting to create an XML file instead, but doesn't matter (per LinkedIn course)
- There is an emulation package from Intel (I'm running on AMD though) that makes testing run faster called HAXM. _HAXM fails to install_ and it's possible that it's AMD? Or maybe some other reason. **_Still able to run the simulation._**


**Run the app:**
- Connect the old phone (Samsung GalaxySiii) via USB, and then put it in Developer mode (per the Dev guide)
- The app RUNS when selected!

<img src="/images/app-hello-world.png" width="30%">


TODO:
- Install "empty" app
- Add button (how?)
- Make the button toggle _something_
- 
- Test the simulation to turn its **flashlight on**
- [Kotlin course](https://developer.android.com/courses/android-basics-kotlin/course)
- Create an app that does one thing, flashlight on and off, and then make it beep high and low pitches.
- [Beginner flashlight app (java)](https://www.geeksforgeeks.org/how-to-build-a-simple-flashlight-torchlight-android-app/)
- [Android hardware LightState reference](https://developer.android.com/reference/kotlin/android/hardware/lights/LightState)
- [camera2/CameraManager](https://developer.android.com/reference/kotlin/android/hardware/camera2/CameraManager)
- A bit more advanced for [Camera2/CameraManager](https://tylerwalker.medium.com/integrating-camera2-api-on-android-feat-kotlin-4a4e65dc593f)

#
[Fundamentals documentation](https://developer.android.com/guide/components/fundamentals)
#

##







### And now that we've installed a functional app on our Android...

(Finish flashlight and beep in the app.)

##### keys to BLE:

1. **Link to repository: [KABLE](https://github.com/JuulLabs/kable)** "Kotlin Asynchronous Bluetooth Low Energy provides a simple Coroutines-powered API for interacting with Bluetooth Low Energy devices."
2. Here is **Android BLE [guide](https://punchthrough.com/android-ble-guide/)** which might help
3. [Android hardware **BLE** device reference](https://developer.android.com/reference/kotlin/android/companion/BluetoothLeDeviceFilter.html)
4. [<usesfeature>](https://developer.android.com/guide/topics/manifest/uses-feature-element)

TODO:
- Enable app to turn on flashlight with a button, and then make it work with BLE external prompt


#

#

[*[ top ]*](GolfSwingSensors.md/#golf-swing-sensors)
[*[ 1 ]*](implementation.md/#the-accelerometer)
[*[ 2 ]*](implementation.md/#solve-for-power)
[*[ 3 ]*](EnablingBLE.md#step-three)
[_[ 4 ]_](#step-four)
[*[ Reference ]*](activity.md/#reference)
[**_Next: [ 4.5 KWS ]_**](KWS.md/#key-word-spotting)
[*[ 5 & 6 ]*](activity.md/#steps-five-and-six)
[_[ notes ]_](thoughtsandnotes.md/#other-projects)
