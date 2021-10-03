# Documenting my activity:
**Learning about this:**
- **How to edit Arduino sketches!**
- GitHub Desktop, KiCad, LTspice, OSHPARK, probably GitBASH
- Arduino Components and code requirements

[_(jump to --> Implementation)_](#implementation)
## Completed:
1. **Git** Practicing with Repositories, Branches, Commits, Pull Requests, and Merges with [help from this Guide](https://guides.github.com/activities/hello-world/)
2. **Markdown** Learned to use [Markdown from this guide](https://guides.github.com/features/mastering-markdown/)
3. **TinyML** Took course for TinyML from EdX ([complete](https://credentials.edx.org/credentials/4601700d748c4de0b3a57315ff04831c/))
4. **activity.md** Describes my next steps, ideas, and this Completed list (now is a [to-do list](activity.md))
5. **golf-swing-idea.md** Defines the parts of my golf swing idea (now incorporated into README)
6. **GitHub Pages** Added using [this guide](https://guides.github.com/features/pages/) and is accessible _here_ (ongoing)
7. Changed _hello-world_ (this) repository to PRIVATE, so it now isn't publicly available on GitHub Pages either
8. Installed **GitHub Desktop**, and added [_ArduinoBLE-to-Android_](https://github.com/jeffreysorgen/ArduinoBLE-to-Android) repository (now may not be needed)
9. Defined Golf Swing Data Flow, [_below_](#flow-for-golf-swing-sensors)
10. [Part One:](#part-one-the-magnetometer) **Set up Magnetometer**
11. Created _new-readme.md_ (now [README](README.md)), added implementation.md (this) and made [activity.md](activity.md) into a TO-DO list
12. Renamed (this) repository: _hello-world_ is now _golf-swing-sensors_, and updated [README.md](readme.md)

# Implementation:
### Flow for Golf Swing sensors:
- **BLE** is enabled when the device is powered, waits for pairing forever.
- Pairs with phone app.
- Clear device buffer and **begin loop**
- Enable mag/acc/gyro
- Mag/acc/gyro wait for stillness (no movement)
- Mag then identifies direction of gravity=zero.
- Acc/gyro records movement. 
- **Collection mode:** After movement, enables microphone.
  - **Microphone** listens for 3 seconds, and records Null, unless it hears "yes" (**This is KWS**)
  - Send data points to phone app with label. (_Is collection and inference possible together?_)
- _**Inference mode:** Checks data points with **MODEL**_
  - _Beep/buzz if good swing_
  - _Silent (or low tone) if no match_
## Part One: The Magnetometer
What instrument determines when to begin doing something? The **magnetometer**. It is either ready for a swing, or it is back in the golf bag!
### Use the Magnetometer readings to determine its orientation to get start/rest orientation of the device
- Attach the device to a stick in a perpendicular fashion as shown here. _Imagine your golf club is either being used, or is back in the golf bag._
#### Images: (1)Attach the device to a stick (2)Rest orientation (3)Device orientation (4)Start orientation
(1)<img src="https://user-images.githubusercontent.com/1236972/135545687-3e1b9fda-1544-4802-93a2-572b97b9b99b.png" width="20%">
(2)<img src="https://user-images.githubusercontent.com/1236972/135545966-edb098fe-ab01-4e57-8c80-1988ad779186.png" width="20%">
(3)<img src="https://user-images.githubusercontent.com/1236972/135546061-106e68f4-bfba-4cd5-929e-49494486ad87.png" width="20%">
(4)<img src="https://user-images.githubusercontent.com/1236972/135545934-7cb4dd34-7c12-46b9-ae8f-fa2e61835812.png" width="20%">
- The graph(3) between the rest(2) and start(4) orientation shows the difference in the readings of the Y axis when the device is attached _"the long way"_ where the red Y-axis reading is always greater than both or less than both other axes. When the "stick" is in neither the start nor rest position, the Y-axis is not significantly separated from the X and Z readings.
- The readings of the magnetometer, according to the setup in the images: 
  - If `Y < X`  and  `Y < Z`  then its orientation is in the start position. So if the y axis readings are less (or more negative) than the other two readings, then the device is in the start position.
  - The opposite is true. If Y is greater than the other 2 axes, then the device is idle and in rest, **and is meant to WAIT FOREVER for its orientation to return to the start position.**


    - BUT THIS DOES NOT HAPPEN MID-SWING while gyro/acc are engaged (_Fix this by disengaging mag mid-swing?_)


###
- **Created _test_IMU_custom.ino_ sketch**
  - Slow down the sensor (_do we need so many data points? don't do this at all?_) 
    - In the SETUP loop, change the baud rate to lowest --> `Serial.begin(300);` <-- from 9600
    - Determine whether to leave baud at 9600, and LEARN to take fewer sample data points (rather than just slowing it down)
  - **Add this to repository? Where is the local file if it's in GitHub locally? Where is it now?**
# [NEXT STEPS -->](activity.md)
### Edit SKETCH and upload: to display "Ready" and/or "Resting" in the Serial Monitor
[<-- back to README.md](readme.md)
