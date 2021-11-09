AppDev.md

#
- Android Studio

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

**To do:**

1. Develop the Hello World of Android Apps
2. Add BLE capability
3. Move on to KWS

##### Development:

- Enable smartphone functions with Android Studio 
- Try and utilize a virtual environment. _(not for this)_
- Enable the app to act upon a BLE characteristic changing from 0 to 1
- Here is **Android BLE [guide](https://punchthrough.com/android-ble-guide/)**

##

- Install Android Studio. (done)
- Buy _motion.golf_ domain. (done)
- [Build first app.](https://developer.android.com/training/basics/firstapp)
  - Create vector icon
  - _WorkManager_ performs background tasks (so implement this for BLE connectivity?)
  - _Using minimum version API 19_ (defalut was 21) because of my old Android
- Enable GitHub connection:
  - Enable local authentication

<img src="/images/gitloginlocal.PNG" width="50%">

- "/images/gitloginlocal.PNG"

- [Kotlin course](https://developer.android.com/courses/android-basics-kotlin/course)

#

#

##

### And now that we've installed a functional app on our Android...

##### the BLE key:

**Link to [KABLE](https://github.com/JuulLabs/kable)**
- "Kotlin Asynchronous Bluetooth Low Energy provides a simple Coroutines-powered API for interacting with Bluetooth Low Energy devices."

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
