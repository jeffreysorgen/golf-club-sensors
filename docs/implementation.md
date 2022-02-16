
[*[ back to overview ]*](GolfSwingSensors.md#golf-swing-sensors)
[*[ completed ]*](completed.md#completed)
[*[ learning ]*](completed.md#learning-about-this)
[_[ **Step One: The Accelerometer** ]_](#the-accelerometer)
[*[ Step Two: Solve for Power ]*](#solve-for-power)
[*[ Step Three: Enabling BLE ]*](EnablingBLE.md/#enabling-ble)

## Implementation





##
##### Step One:
# The Accelerometer

**Description:**

**The goal is to "turn on" readings when sensor is oriented with clubhead down at the ground.**
What instrument determines when to begin doing something? 
**The accelerometer**. 
Depending on how the device is attached, one axis determines the direction of gravity. 
That axis has an approximate reading of 1G, the force of gravity in one direction.
Resting state is the negative value of its Ready state.
Some combination of coordinates might need to be calculated with advanced math for _gravity=zero._
But at this stage of development it probably isn't necessary.

**Why the Accelerometer?**

I need one reading - _knowing the orientation of the device._ 
Since the Accelerometer +/- Z axis is up/down, then this is the sensor to use. 
Depending on how the device is attached, it's one of the three axes. 
It's mounted flat and perpendicular to the stick, so the axis changes to the Y-axis.
The axis is "opposite" when the club handle is pointing down and in Resting state. 
When the handle is upright, the club is in play and the sensor is in Ready state.

**For energy conservation:**

First, understand the orientation of the device.
When the sensor identifies its orientation is as if the club has been put back in the golf bag, then the device just uses one accelerometer parameter, `y < -.85`, meaning Resting state, and then continues to check every two seconds for a change in state. 
BLE will remain engaged.
The connected smartphone device will **beep** only when it identifies a _state change_.
The _beep_ at this point is for development purposes, and is intended for future use elsewhere.

### Setting up the device:
These readings will determine the Ready or Resting state orientation
- For ease of use, put the Arduino into a breadboard and then attach it to a stick in a perpendicular fashion as shown here. 
- _Imagine your golf club is either being used, or has been put back in the golf bag._

##### Images: (1)Attach the device to a stick (2)Ready orientation (3)Device orientation (4)Resting orientation
(1)<img src="/images/Sm-attaching to a stick.png" width="20%">
(2)<img src="/images/Sm-start orientation.png" width="20%">
(3)<img src="/images/Sm-device orientation.png" width="20%">
(4)<img src="/images/Sm-rest orientation.png" width="20%">

**The readings of the accelerometer, according to the setup in the images:**

There is a difference between the Ready(2) position and the Resting(4) orientation. When the device is attached as shown(1), one parameter, the Y-axis (in red) of the Accelerometer, tells the system whether it's in Ready state or it's in Resting state. When the Y-axis reading is positive then the sensor is in one state, and when it's negative it's in the other. Attached this way, the graph(3) shows that the Ready state is positive and the Resting state is negative.

**Notes about Resting state:**

The Resting state is meant for when the club is in the bag. If it's in the bag then it's not going to take readings. That would be wasteful. So it's meant to pause all the readings before any more readings are taken. The sensor will stay in Resting state until it senses Ready state. Once in Ready state, the other sensors are turned back on again.

**What happens:**

1. powered on
2. in the bag, so Resting state
3. pulled out of the bag, senses Ready state
4. at this point, **waits to settle** so it can begin recording motion [(in the next (gyro) section)](activity.md#collect-gyro-data)

#### As shown in the serial monitor:
- The Resting state:
  - the y-axis reading is near -1G (_-0.85_) and displays "One second delay..." 
  - then checks every 1 second using `delay(1000)`
- When `y > -.85`, then the Monitor shows "Ready!" 
  - and displays all the sensor readings (currently just acc)

<img src="/images/one second delay.PNG" width="50%"/>

### Updating the Arduino Nano 33 BLE
- **Find** the _SimpleAccelerometer_ sketch from the Example files in the **Arduino_LSM9DS1 folder**
- IMPORTANT: **Save it as** _golf-swing-acc_
- **Add the _if-else_ statements within the _void() loop_ as shown.** (the rest is unchanged) 
  - The _if/else_ statement creates the _-0.85_ threshold between the Ready and Resting states.

#### Code for the new _LOOP_ is here:
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

## Summary:
This section was about setting up the Accelerometer, physically and with the IDE, so that it performs as expected.
The goal was to basically create on/off states, accomplished here by using a threshold for the Ready and Resting states. 

**Exception to this configuration:**

Swinging the club around won't put it into that Resting state unless it registers that particular state of inertia below _-0.85_. While there may be a risk of hitting that threshold while the club is in play, some cursory testing shows that it's possible the risk is low and `(y<-0.85)` doesn't happen or it doesn't hit the delay for some reason. **This exception has been resolved in later code.**

##
##### Step Two:
# Solve for Power

#### Future prototyping solution

- Later can build an obviously better solution.  
- When 100% finished developing with my Arduino Nano 33 BLE Sense, I will be looking into using a different board for prototyping, and a battery solution will definitely be a part of the research.
  - **The board needs to include (1) an IMU, (2) a microphone, and (3) a solvable battery option**
- Battery options:
  - I am looking for those **2-prong** "magnetic" battery chargers, what kind of battery is in that fit-watch, and where to get that rechargable battery. 
  - **Qi coil** is a wireless charging device.
  - **CR1220** is a small, common coin-type battery
  - **LIR2032H** is a common 3.7 rechargable, but 20mm, so like a nickel size.

#### Current development solution

- **Connect with only BLE and be _physically detached_ from the computer.**
- Attach the Arduino Sense (USBmicro port) to a power source.
- Use a lightweight **phone recharger** (with 2 USB out) to serve this purpose during development.  
- Rechargers will shut off after a short time with just a low power drain.
- **Charging an _old phone_ at the same time will prevent this auto-shutoff** 

##### Charging up a dead old phone, simultaneously powering Nano33BLE

<img src="/images/Sm-batterypack.png" width="35%">

#### Alternative development solution

- This is not practical for _golf-club-sensors_ project but is helpful information nonetheless.
- There's a power solution in the TinyML Course, attaching a 9V battery to the **Learning Kit Shield**. 
  - This [**Appendix from the TinyMLx repo**](https://github.com/tinyMLx/appendix/blob/main/PoweringArduino.md#battery) is a good place to read about it.
  - It's certainly not designed for swinging around, but it is proof that there's a pinout solution.
  - It's a good example for a stationary device.

#
[*[ Top ]*](GolfSwingSensors.md#golf-swing-sensors.md)
[*[ Step One ]*](#step-one)
[*[ Step Two ]*](#step-two)
**Next: [*[ Step Three: Enabling BLE --> ]*](EnablingBLE.md/#step-three)**
[*[ 4 ]*](AppDev.md/#step-four)
[*[ 5 ]*](activity.md/#steps-five-and-six)

