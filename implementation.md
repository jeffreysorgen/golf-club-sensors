# Documenting my activity:
**Learning about this:**
- GitHub Desktop (don't mess it up!), possibley GitBASH
- IMU sensors, and Fourier Series, until I understand IMU aspects about a golf swing
- **How to edit Arduino sketches!**
- KiCad, LTspice, OSHPARK
- Arduino Components and code requirements

[_(jump to --> Implementation)_](#implementation) [_(open for --> Discussion!)_](https://github.com/jeffreysorgen/golf-club-sensors/discussions)

## Completed:

_skip to
[\[Sept\]](#2021-sept)
[\[Oct\]](#2021-oct)
2021_

##### _2021 Sept_ 

1. **Git** Practicing with Repositories, Branches, Commits, Pull Requests, and Merges with [help from this Guide](https://guides.github.com/activities/hello-world/)
2. **Markdown** Learned to use [Markdown from this guide](https://guides.github.com/features/mastering-markdown/)
3. **TinyML** Took course for TinyML from EdX ([complete](https://credentials.edx.org/credentials/4601700d748c4de0b3a57315ff04831c/))
4. **activity.md** Describes my next steps, ideas, and this Completed list (now is a [to-do list](activity.md))
5. **golf-swing-idea.md** Defines the parts of my golf swing idea (now incorporated into README and deleted)
6. **GitHub Pages** Added using [this guide](https://guides.github.com/features/pages/) and is accessible _here_ (ongoing)
7. Changed _hello-world_ (this) repository to PRIVATE, so it now isn't publicly available on GitHub Pages either (Now renamed to _golf-swing-sensors_ and **is** public. GHPages not enabled.)
8. Installed **GitHub Desktop**, and added [_ArduinoBLE-to-Android_](https://github.com/jeffreysorgen/ArduinoBLE-to-Android) repository (now may not be needed)
9. Defined Golf Swing Data Flow, [_below_](#flow-for-golf-swing-sensors)
10. Created _new-readme.md_ (now [README](README.md)), added implementation.md (this) and made [activity.md](activity.md) into a TO-DO list. (Now deleted the old readme-old.md.)
11. Renamed (this) repository: _hello-world_ is now _golf-swing-sensors_, and updated [README.md](readme.md)
12. [Implementation Part One:](#part-one-the-accelerometer) **Set up Accelerometer**

##### _2021 Oct_

13. Determined that the Magnetometer is _not_ going to be used for Ready/Resting orientation. _Gained understanding._ Learned that Accelerometer is going to be used, and magnetometer is unnecessary for the project.
14. Determined that it is _better_ to use `millis()` rather than `delay()`
15. Figured out how to set up Ready/Resting states in Arduino Sketch, and display in Monitor. (in progress)
16. LEARNED that it's easy to mess up in GitHub desktop, so _be careful_
17. LEARNED Cursory principles of electronics from __LinkedIn Learning__.


# Implementation:

### Flow for Golf Swing sensors:

- **BLE** is enabled when the device is powered, waits for pairing forever.
- Pairs with phone app.
- Clear device buffer and **begin loop**
- Enable mag/acc/gyro
- Mag/acc/gyro wait for stillness (no movement)
- Mag then identifies direction of gravity=zero. (_edit this_)
- Acc/gyro records movement. 
- **Collection mode:** After movement, enables microphone.
  - **Microphone** listens for 3 seconds, and records Null, unless it hears "yes" (**This is KWS**)
  - Send data points to phone app with label. (_Is collection and inference possible together?_)
- _**Inference mode:** Checks data points with **MODEL**_
  - _Beep/buzz if good swing_
  - _Silent (or low tone) if no match_


## Part One: The Accelerometer

#### *Rewrite this entire section*
- This section will be rewritten. 
Graphic of mag sensor readings should be swapped out for accelerometer readings.
Push towards understanding what to do with the acc readings. 
Need to identify more specific steps for this project phase.
Might need to rewrite the following documentation/progress so that it's more accurate.

#### Rearrange this page:
- First part is assembling the device on the stick.
- Second part is the Arduino IDE.
- Third, make the monitor show "Ready" state, and that's the end of this page, because of clarity about the end of the section. 


#
### Description: 

**The goal is to "turn on" readings when sensor is oriented with clubhead down at the ground.**
What instrument determines when to begin doing something? 
The **accelerometer**. 
Depending on how the device is attached, one axis determines the direction of gravity. 
That axis has an approximate reading of 1G, the force of gravity in one direction.
Resting state is the negative value of its Ready state.
Some combination of coordinates might need to be calculated with advanced math for _gravity=zero._
But at this stage of development it probably isn't necessary.

#### Why the Accelerometer?
I need one reading - _knowing the orientation of the device._ 
Since the Accelerometer +/- Z axis is up/down, then this is the sensor to use. 
Depending on how the device is attached, it's one of the three axes. 
It's mounted flat and perpendicular to the stick, so the axis changes to the Y-axis.
The axis is "opposite" when the club handle is pointing down and in Resting state. 
When the handle is upright the club is in play, and the sensor is in Ready state.

#### For energy conservation: 
First, understand the orientation of the device.
Then, make the device **beep** only when it identifies a _state change_.
After two minutes in Resting state, the state changes to Timeout state, and halts other sensors.
(To-do: Technical description of Timeout state.)
It then listens only for Ready state.



## Accelerometer: Setting it up

### Use the Accelerometer readings to determine its orientation to get start/rest orientation of the device
- Attach the device to a stick in a perpendicular fashion as shown here. _Imagine your golf club is either being used, or is put back in the golf bag._

#### Images: (1)Attach the device to a stick (2)Rest orientation (3)Device orientation (4)Start orientation
(1)<img src="https://user-images.githubusercontent.com/1236972/135545687-3e1b9fda-1544-4802-93a2-572b97b9b99b.png" width="20%">
(2)<img src="https://user-images.githubusercontent.com/1236972/135545966-edb098fe-ab01-4e57-8c80-1988ad779186.png" width="20%">
(3)<img src="https://user-images.githubusercontent.com/1236972/135546061-106e68f4-bfba-4cd5-929e-49494486ad87.png" width="20%">
(4)<img src="https://user-images.githubusercontent.com/1236972/135545934-7cb4dd34-7c12-46b9-ae8f-fa2e61835812.png" width="20%">

(**Edit needed**) The graph(3) between the rest(2) and start(4) orientation shows the difference in the readings of the Y axis when the device is attached _"the long way"_ where the red Y-axis reading is always greater than both or less than both other axes. When the "stick" is in neither the start nor rest position, the Y-axis is not significantly separated from the X and Z readings.

(**Possible edits:**) One parameter, the Y-axis of the Accelerometer, tells the system whether it's in Ready state or Resting state. When Y-axis reading is positive then the sensor is in one state, and when it's negative it is in the other.




#### The readings of the accelerometer, according to the setup in the images: (edit needed) 

- (**Possible edits needed?**) If `Y < X`  and  `Y < Z`  then its orientation is in the start position. So if the y axis readings are less (or more negative) than the other two readings, then the device is in the start position. More simply, Z axis is positive, or Z axis is negative. (and if the device is attached "sideways" then it's a different axis)

**Edit this part:**
- The opposite is true. If Y is greater than the other 2 axes, then the device is idle and in rest, **and is meant to WAIT FOREVER for its orientation to return to the start position.**



## To do:

### Do the SimpleAccelerometer sketch _again_ in the IDE:

- Save as _golf-sensors.ino_ 
- Do standard test. Watch the plot. - Determine whether to delete X and Z in the sketch, because only Y is needed
- Set up if statements `{ if Y < -.8 then it is Resting state }`
  - Do not allow Resting state during the SWING (figure it out)
    - There is no risk of this while determining orientation (Ready/Resting) (?)
    - The risk happens while waiting in Ready state to record data (?)
- Stop printing data while in Resting state
- But start printing data back in Ready state
- **Take new illustration photo(3).**



### Create new custom sketch
(MERGE THIS STUFF WITH ABOVE STUFF)
- Local file:
  - Add this to repository? Where is the local file if it's in GitHub locally? Where is it now?


# [NEXT STEPS -->](activity.md)
### Edit SKETCH and upload: to display "Ready" and/or "Resting" in the Serial Monitor
[<-- back to README.md](README.md)
