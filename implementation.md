
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
18. **Arduino sketch** baseline created for IMU - _golf-swing-acc_ ([ongoing improvements](#updating-the-arduino-nano-33-ble)) and also, **made sure not to overcomplicate it** and kept it SIMPLE
19. **GitHub Markdown** Learned styling for tables, images and code block
20. DOCUMENTED progress towards **battery** solution [_here_](activity.md#battery-info) (ongoing)


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
Then, make the device **beep** only when it identifies a _state change_ (for development purposes only).
When the sensor identifies orientation is as if the club has been put back in the golf bag, then the device just uses one parameter in the accelerometer, `y < -.85` meaning club is in Resting state, and continues to check every second for a change with `delay(1000)`. 
BLE will remain engaged though.

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

#### Notes about Resting state
What is Resting state meant for? It's meant to sense when the club is in the bag. If it's in the bag then it's not going to take readings. That would be wasteful. So it's meant to pause the readings. To pause the readings, there is a `delay(1000)` before more readings are taken. The sensor will be in Resting state until it senses Ready state. Once in Ready state, other sensors are turned on. (working this way now)


#
### So... 
1. powered on
2. in the bag, so Resting state
3. pulled out of the bag, senses Ready state
4. at this point, **waits to settle** so it can begin recording motion
##### The way this should work is:
- Enable Resting state. (This is in the code already)
  - Serial.Monitor shows "Resting..." because `y < -.85`
  - and checks every second with `delay(1000)`
- When NOT `y < -.85`, then Serial.Monitor shows "Ready!" 
  - and displays all the sensor readings (currently just acc)
### Basically...
1. Device is powered up and attached to a club in the golf bag
2. `( y < -.85 )` so the only thing it's doing is waiting with `delay(1000)`
3. Then once it's not true anymore, it goes back to Ready state and does everything else
#





## Updating the Arduino Nano 33 BLE
- Inside the _LOOP_, we added `if ( y > -.85 )` to establish the Ready state threshold, and pauses within the `else` statement for one second when it's in the Resting state
- The _if/else_ statements establish thresholds for the Ready and Resting states.
  -  _**Ready**_ is always when the club is in play.
  -  _**Resting**_ is when the sensor reads that its orientation is negative (-.85) 

### Edit the sketch. 
- Open _SimpleAccelerometer_ from the Example files in the Arduino_LSM9DS1 folder **and Save as...** _golf-swing-acc_
- There are no changes except for within `void loop()`
- In `void loop()`, add the _if/else_ statements as shown here 
#### Here's the new _LOOP_:
```
void loop() {
  float x, y, z;

  if (IMU.accelerationAvailable()) {
    IMU.readAcceleration(x, y, z);
    if ( y > -.85 ) {  // -1G is the threshold
      Serial.print("Ready!");
      Serial.print('\t');
      Serial.print("X = ");
      Serial.print(x);
      Serial.print('\t');
      Serial.print("Y = ");
      Serial.print(y);
      Serial.print('\t');
      Serial.print("Z = ");
      Serial.println(z);
      }
    else { 
      Serial.print("One second delay...");
      Serial.print('\t');
      Serial.print("Y = ");
      Serial.println(y);
      delay(1000); // one second delay
      }
    }
  }
```

## Watch the Serial Monitor:
<p align="center"><img src="images/one second delay.PNG" width="25%"/></p>

## To do:
- **Set up device precisely as described on this page**
- **Screenshot** the Serial Monitor display for an example (done but not here yet) 
- AND FINALLY, **verify** that what's in the [Conclusion](#conclusion) is actually accomplished and true (true, but exception has been noted)
#
### Sections of this page:
- First part is assembling the device on the stick. (done)
- Second part is the Arduino IDE. (code is pasted here)
- Third, make the monitor show "Ready" state. (got screenshot, must paste)
- Finally, because of the clarity of the section so far, that's the end of this page. (until BLE is fully written)
#
## Conclusion:
This section was about setting up the Accelerometer, physically and with the IDE, so that it performs as expected.
The goal was to basically create on/off states, accomplished here by using a threshold for the Ready and Resting states. 

### This phase is done when I can show the expected results:
- When the golf club is in its bag, then little energy is spent because `delay()` is being used until it senses that it's in the Ready state
- While the golf club in the Ready position, all the readings are streaming through (acc readings for now)
- Swinging the club around won't put it into that Resting state unless it registers that particular state of inertia below _-0.85_. 
  - While there may be a risk of hitting that threshold while the club is in play, some cursory testing shows that it's possible the risk is low and `(y<-0.85)` doesn't happen or it doesn't hit the delay for some reason.
- Before moving forward to **BLE** section, it's important that the Sketch has been created 100% as planned, the code documented, and the resulting Serial Monitor screen shown as an example. (done)

# [NEXT STEPS -->](activity.md)
### Implementing BLE
[<-- back to README.md](README.md)
