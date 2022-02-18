
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
4. at this point, **waits to settle** so it can begin recording motion [(in the next (gyro) section)](#step-four-gyro)

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


### Summary

We have enabled the Accelerometer and found a temporary solution for power.

**Next**, we're going to find a way to connect the device to a smartphone utilizing BLE,
which will be useful in the future when a smartphone application is being built.
In the process of implementing BLE, we're going to examine the structure of the Arduino code.
Finally, we'll come back to the Accelerometer and fine tune it to more closely match our use case
of determining whether the club is in the golf bag or being used.



##
##### Step Three:
# Enabling BLE

**Description:**

Before we can get it to chirp in response to a good or bad swing, the smartphone needs to pair up with the Arduino BLE Sense. We should be able to see on my Android whatever information we've already sent to the serial monitor. To do this, Nordic has an app we can download from Google Play called _nRF Connect_. Let's go through the process of getting that started right here. 

### Try the BLE example sketch

I found [_**this video**_](https://youtu.be/2q_tA8v5l1Y) from _Robocraze_ to be helpful, 
and copied the code from its [**accompanying GitHub repository**](https://github.com/Robocraze/Nano-33-BLE-Examples/blob/43fbe5b3155493d3056e85d7402c54e05c84f133/environment_sensor_ble/environment_sensor_ble.ino).
This example reads information from the sensors and then simply displays it in the phone app. 
Upload the sketch to the device... _**And it works exactly as it does in that video.**_ 
_But there is a [**caveat**](#caveat) which I discovered for this example._ 

**To do this:**

1. Download _nRF Connect_ from **Google Play** (also available for iOS)
2. Find the `environment_sensor_ble.ino` file from the _Robocraze_ repository and copy it locally. 
_(Right-click on _Raw_, save the file, and drop into same-name folder, as required by Arduino.)_
3. Upload the _environment_sensor_ble_ sketch to the device
4. Open the serial monitor, watch for "Disconnected from central..."
5. (App) Open the phone app, _nRF Connect_, and enable Bluetooth
6. (App) Scan for and connect to the device (its name was declared in the sketch)
7. (App) Touch _UnknownService, UUID: 0x180C_ (_"180C"_ is unregistered generic UUID)
8. Watch Monitor again for services to pop up
9. (App) Touch the "triple down arrow" <img src="/images/3downarrows.png" width="20em" /> for each of the three services for this example
10. (App) OBSERVE the temperature gradually reach ambient room temperature or hold in hand for it to rise

##### Caveat:

With the USB cable plugged into the computer I **_can_** discover _"Arduino Environment Sensor"_ in nRF Connect. 
But a [**battery-only**](implementation.md/#current-development-solution) solution can **_not_**. So...

**For battery-only:**

- Comment out `//while (!Serial);` 

After being untethered from the computer, the device was trying to find the serial port from which it's now disconnected.
So this one change will allow the device to function in _nRF Connect_ the same way as it did before.

### The Hello World BLE Sketch

Now that we've got the BLE connecting, and IMU data showing up in _nRF Connect_, it's time to simplify and specialize our code.

There is a _BLE Hello World_ sketch from [okdo.com](activity.md/#reference) 
that turns on the amber LED on the Arduino board when it connects, 
and we can then read "Hello World" on the Client (smartphone) app.
So starting with this simple code as a base, **we'll combine it with our own _golf-swing-acc_ sketch** 
so that we can see what gets sent from the Nano33BLESense.

- View the _Hello World BLE code_ [**here**](#ble-hello-world-code-is-here)

The big takeaway with this code is that the `while (central.connected())` command just hangs the activity until BLE disconnects and then the code turns the LED off. 
(It's LED-centric code.) 
The LED will be useful for indicating "Ready" and "Resting" states.

We've seen in those two examples what a basic `.ino` file looks like. **Here's a summary:**

### Arduino File Structure

Here we will describe the very basic structure of an Arduino `.ino` file. 

**At the most basic level, there are four sections:**

1. *"prior to"*
2. `void setup()`
3. `void loop()` and
4. *"other functions"*

**1. prior to `void setup()`**

- These can be within _namespace_
- First add LIBRARIES
- Initialize and name CONSTANTS
- Initialize VARIABLES
- Initialize BLE SERVICES
  - Give the Services and Characteristics their UUIDs ([here](activity.md/#uuid-info) for more info)
- Initialize respective BLE Service CHARACTERISTICS
- Create the FUNCTION PROTOTYPE (which reside in "other functions" area)

**2. `void setup()`**

- Initialize the SENSORS
- Initialize SERIAL COMMUNICATION
- Initialize OTHER things (such as built-in LED pin)
- Check for FAILURE
- Create the BLE NAME to show up in the SCAN
- Create the BLE SERVICE for advertising
- Create the BLE Service CHARACTERISTICS
- ADD the BLE SERVICE to advertise
  - The variable names were previously initialized above
- ADVERTISE BLE
- Set VALUES for strings or constants
  - These variables were previously initialized above
  
**3. `void loop()`**

- conditional settings and calculations repeated within the loop
- _if-else_ statements, etc
- engaging LEDs, etc

**4. other functions**

- subroutines _(and other stuff)_

### Creating the new code

**Importing new functionality into our code:**

We're now going to take what we've learned from our two examples and incorporate them into our feature code, now called **_golf-swing-acc-ble_**.

- View the combined code [**here**](#all-the-golf-swing-acc-ble-code-is-here)

##### Phone screen with device listed: (1) scanning, (2) connected, (3) tilting on the y-axis to turn on/off the LED

<p align="center">
  (1) <img src="/images/BLEScanning.png" width="20%">
  (2) <img src="/images/BLEConnected.png" width="20%">
  (3) <img src="/images/myBLEtilt.gif" width="30%">
</p>

##

**Almost done**

Although things are working well, there are still two things we should improve upon. 
One tweak is to accommodate for an unintentional state change from a bounce of the sensor, 
and the other is to reduce the amount of BLE communication, sending only once at the moment of a state change.

So we created **_golf-swing-acc-ble-statechange_** with this modified code: 
- Eliminate accidental state changes from the sensor
- Send data via BLE **only** when the state changes 
- Send _boolean 1/0_ rather than the strings, "Resting" and "Ready"
- Use the shorter 16-bit UUID, like `ffe0` and `ffe1` [_(More about UUID)_](activity.md#uuid-info)

##### BLE sends data only when the words "State change to" appear

<img src="/images/stateshanges.gif" width="80%">

## Summary:

**Server/peripheral-side BLE programming is done for now.**

We started with physically setting up the Arduino Nano33BLESense as if it were attached to the back of a golf club head.
Then we implemented the code to be able to see the readings of the Accelerometer in the Serial Monitor screen.
After experimenting with a couple of example sketches, we incorporated the BLE library into the code, downloaded the nRF Connect application to a smartphone, and were **able to see readings** coming in. 

The Nano33BLESense has now been programmed to communicate with a Client (central), so it's time to develop an Android application that it can control, basically with an on/off signal sent through Bluetooth Low Energy. 
(This idea was based on being less time consuming and being easier for app development.)
**Enabling BLE communication with a smartphone will instead be shifted out to a step after Data Collection.**
_Rather than focusing immediately on the smartphone application, we should collect data on the device which can then be sent via BLE._

The section above describes two things: One, reading data from the Accelerometer, and two, setting up one side of a BLE connection.
**Both are important.**
Reading the Accelerometer is important because it determines whether the golf club is actually being used (is in Ready state).
And BLE will need to be used to send swing data to a smartphone _or computer_ because there's only 256k available within the Device.

**The next step is to collect and record swing data from the Gyroscope.**

##
##### Step Four (gyro)
# Collecting Gyro Data

**Description:**

The Accelerometer has been used to determine whether a golf club is being used or has been put back in the golf bag, based upon its orientation.
When in play, it is able to begin recording the motion of the Gyroscope.



[Link to Gyro Steps](activity.md/#steps-five-and-six)



#
[*[ Top ]*](GolfSwingSensors.md#golf-swing-sensors.md)
[*[ Step One ]*](#step-one)
[*[ Step Two ]*](#step-two)
[*[ Step Three ]*](#step-three)
[*[ 4 ]*](AppDev.md/#step-four)

