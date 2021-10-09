
# Documenting my activity:

**Learning about this:**
- IMU sensors, **How to edit Arduino sketches!** C++
- GitHub Desktop, possibly GitBASH, KiCad, LTspice, OSHPARK
- Arduino Components and code requirements

[_[ jump to --> Implementation ]_](#implementation) [_[ open for --> Discussion! ]_](https://github.com/jeffreysorgen/golf-club-sensors/discussions)

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
18. **Arduino sketch** baseline created for IMU - _golf-swing-acc_ (ongoing)


# Implementation:

### Flow for Golf Swing sensors:

- **BLE** is enabled when the device is powered, waits for pairing forever.
- Pairs with phone app.
- Clear device buffer and **begin loop**
- Enable mag/acc/gyro
- Mag/acc/gyro wait for stillness (no movement)
- Acc then identifies direction of gravity=down.
- Acc/gyro records movement. 
- **Collection mode:** After movement, enables microphone.
  - **Microphone** listens for 3 seconds, and records Null, unless it hears "yes" (**This is KWS**)
  - Send data points to phone app with label. (_Is collection and inference possible together?_)
- _**Inference mode:** Checks data points with **MODEL**_
  - _Beep/buzz if good swing_
  - _Silent (or low tone) if no match_


## Part One: The Accelerometer

### Description: 
**The goal is to "turn on" readings when sensor is oriented with clubhead down at the ground.**
What instrument determines when to begin doing something? 
**The accelerometer**. 
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
When the handle is upright, the club is in play and the sensor is in Ready state.

#### For energy conservation: 
First, understand the orientation of the device.
Then, make the device **beep** only when it identifies a _state change_.
After two minutes in Resting state, the state changes to Timeout state, and halts other sensors.
It then listens only for Ready state.
- Ready state listens for Resting state (inverse of Ready state at any point) X 240 readings, and waits 250millis each time (2 minutes total) before setting up Timeout state (_edit this_)
- Timeout state waits 30,000 millis (half minute) and listens for Ready state
- _Should use `millis()` rather than `delay()`_

### Setting up the device:
**These readings will determine the Ready or Resting state orientation**
- For ease of use, put the Arduino into a breadboard and then attach it to a stick in a perpendicular fashion as shown here. _Imagine your golf club is either being used, or is put back in the golf bag._

#### Images: (1)Attach the device to a stick (2)Rest orientation (3)Device orientation (4)Start orientation
(1)<img src="https://user-images.githubusercontent.com/1236972/135545687-3e1b9fda-1544-4802-93a2-572b97b9b99b.png" width="20%">
(2)<img src="https://user-images.githubusercontent.com/1236972/135545966-edb098fe-ab01-4e57-8c80-1988ad779186.png" width="20%">
(3)<img src="https://user-images.githubusercontent.com/1236972/135546061-106e68f4-bfba-4cd5-929e-49494486ad87.png" width="20%">
(4)<img src="https://user-images.githubusercontent.com/1236972/135545934-7cb4dd34-7c12-46b9-ae8f-fa2e61835812.png" width="20%">

#### The readings of the accelerometer, according to the setup in the images:
In principle, the readings of the Accelerometer are the same as those shown in this graph(3) for the Magnetometer. There is a change between the rest(2) position and the start(4) orientation. When the device is attached as shown(1), one parameter, the Y-axis of the Accelerometer, tells the system whether it's in Ready state or Resting state. When its Y-axis reading is positive then the sensor is in one state, and when it's negative it's in the other.

In this specific case, the graph would show that the Ready state is positive, and the Resting state is negative.
The value of the Resting state reading is close to -1 (such as `y < -0.85`) and then at that point it **will wait _forever_ for its orientation to return to the start position.**

## IDE Code goes here.
- 

## To do:
- **Implement the Timeout state** (see calcs defined above)
- **Set up device precisely as described on this page**
- Paste the `.ino` file here in the CODE section
- **Screenshot** the Serial Monitor display for an example 
- **Describe** the functionality of the Ready/Resting/Timeout states 
- AND FINALLY, **verify** that what's in the [Conclusion](#conclusion) is actually accomplished and true
- The all-inclusive file will be saved as _golf-sensors.ino_ when more sensors are involved. (_send to to-do page_)
#
### Sections of this page:
- First part is assembling the device on the stick. (done)
- Second part is the Arduino IDE. (in progress, paste code)
- Third, make the monitor show "Ready" state. (get screenshot)
- Finally, because of the clarity of the section so far, that's the end of this page. (until BLE is fully written)
#
## Conclusion:
This page/section was about setting up the Accelerometer, physically and with the IDE, so that it performs as expected.
The goal was to basically create on/off states, accomplished here by using thresholds for the Ready/Resting/Timeout states. 

Before moving forward to **BLE** section, it's important that the Sketch has been created 100% as planned, the code documented, and the resulting Serial Monitor screen shown as an example.

#### This phase is done when I can show the expected results:
- Holding the golf club in the ready position shows all the readings streaming through (acc readings for now)
- Swinging the club around won't put it into a Resting state if it's not really meant to be there, because thresholds have been set up to prevent it from happening
- But once it gets to the Resting state, it changes to the Timeout state and then checks for a state change only every 30 seconds
- When it finds the Ready state again, the process repeats! 

# [NEXT STEPS -->](activity.md)
### Implementing BLE
[<-- back to README.md](README.md)
