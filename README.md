# Golf Swing Sensors Overview
This repository consists of code and any related descriptive narrative about the implementation of my Golf Swing Sensors project. I'm developing this on Arduino Nano 33 BLE Sense, employing TinyML (tfmicro) for inference when needed, and BLE (Bluetooth low energy) for communication between the Nano and a smartphone.

[ Top ](#golf-swing-sensors-overview)[| Accelerometer ](#sensor-accelerometer)[| Power ](#solving-for-power)[| Gyro ](#collecting-gyro-data)[]()[]()[]()

## Background
As an amateur golfer, it's important to get real-time feedback on the condition of my golf swing. I need a way to instantly know whether I am swinging my club correctly prior to hitting the ball, or if I've got it right. For me, the psychology of approaching the ball goes in two directions. One, I am confident and relaxed, or two, my mind is racing and I need to quiet it down.  

What primarily causes me to think too much? I've just flubbed the last shot and I'm trying to get over it. I need to eliminate 100% of the flubs in my game. I would rather concentrate on my aim and club selection decisions, rather than on the goofy swing I am trying to avoid.

Imagine approaching your ball with your club in hand, and taking a few practice swings. In your head, a lot of things are going on based on what you've heard recently from your coach, maybe have seen in coaching videos, or what you heard moments ago from your excellent golf partner.

If you're like me trying to remember what you saw on YouTube, then it's hard to recall the information in a way that's helpful at the very moment you step up to the ball. And your game will be long over when you finally get back to studying how well you did by reflecting on your score as you search for new tips from your favorite sources.

**This project is going to help me with my swing by providing instant feedback when my practice swing is not where it needs to be. I'll instantly know the difference between a good swing and a bad one.** 
- With a good swing I'll hear a positive chirp from my smartphone, which I keep in my golf cart or bag. 
- With a not-so-good swing, I'll hear a sad chirp.

## Functionality of the Device
There are _two_ functions. The first is data collection, described here. Its architecture expects me to say 'yes' following a good swing. 

The other purpose is to use collected data for an ML model that beeps a signal when it identifies a good practice swing. 

**Goal is to implement both functions simultaneously.**

## Physical Description
Attach a small form factor MCU to the back of a golf club head securely to engage movement sensors.

**Physical Development Stages:**

During the first physical stage of development, the device will be directly connected to the computer, having all sensor readings shown in the Serial Monitor. Later the system is configured to collect the data onto an SD card or a folder on the computer.

In the second physical stage of development, a second device (another Nano33BLESense) will be employed, receiving the data transmitted to it by the first device via BLE. This second device is connected to the computer and functions the same way as had already been configured.

The device that's attached to the golf club head will be powered by a small Lithium battery, enabling the sensors and BLE transmission. It needs to store small amounts of data before sending it to the other device and then purging it. Proper swing data can be collected, and based upon an inference it makes, it can signal the second device to trigger its speaker.

The third physical stage for data collection is to remove the computer from the system, having configured the second device to collect data onto an SD card directly if it's a Raspberry Pi, or to an externally attached SD card reader. Any battery solution can be used because the device is stationary.

The fourth physical stage of the system is to entirely replace the second device with a smartphone application. The smartphone can both collect data and make sounds. There is potential opportunity to utilize large scale data collection as well.

**Prototyping:**

- _**First circuit board (the prototype device)**_
  - small form factor, independently powered
  - includes (1) IMU, (2) microphone, (3) BLE, (4) battery
- _**Second circuit board**_
  - any form factor, any battery solution
  - includes (1) BLE, (2) SD card, (3) speaker
  - _(Explore whether MCU emulation [(renode.io)](https://renode.io) is a good alternative to try prior to configuring a second MCU.)_ 

## Technical Description

This is a multi-tenancy model. The IMU and Microphone function in cascade fashion. The BLE is always enabled but only transmits during data collection. _(There's a nuance to this though.)_

**The following describes a way to collect data for a golf swing.** 

It waits for the motion to stop, and then starts looking for the motion of a swing, and then I speak into the microphone 'yes' so that it gains a label, otherwise it is null. 
The null swings are much more frequent, because the system records everything that follows a pause of motion if the orientation of the device is at the starting position.
The BLE service provides a way to record data without restrictions. It's simply always on, without needing to go to a cloud service which is typically necessary for IoT devices, because the smartphone application collects the data. Only the smartphone needs to be near the device, within several feet, and the device does not need to connect to anything else. So this system is ideal for remote functionality, even without cellphone service, because a smartphone can connect via Bluetooth to the device without needing wifi or Cell service.

[ Top ](#golf-swing-sensors-overview)[| Accelerometer ](#sensor-accelerometer)[| Power ](#solving-for-power)[| Gyro ](#collecting-gyro-data)[]()[]()[]()







## Flow:
Uses BLE:
- Start by connecting the MCU to the smartphone
- Begin IMU loop

Uses IMU:
- If Event Data exists, send data via BLE and clear buffer
- IMU waits for zero orientation on plane, 
  * then waits for motion to settle, 
  * then enables recording of swing motion (event loop) 
- record the motion event
  * store motion data
  * engage the KWS 

Uses KWS:
- KWS function listens for 'yes'
- after delay, records null label
- after 'yes' Keyword, records Yes label then returns to IMU input

## A less general description of the flow:
- **BLE** is enabled when the device is powered.
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

_(The above sections should be more succinct and clear.)_

[ Top ](#golf-swing-sensors-overview)[| Accelerometer ](#sensor-accelerometer)[| Power ](#solving-for-power)[| Gyro ](#collecting-gyro-data)[]()[]()[]()







# Sensor: Accelerometer
(Step One)

What instrument determines when to begin doing something? **The accelerometer**. 

The goal is to turn on the sensors when the device is oriented with the clubhead down at the ground and is in play. 
Since the Accelerometer +/- Z axis is up/down, it can determine its orientation. 
Depending on how the device is attached, that axis may change.
When the device is mounted flat and perpendicular to the stick (club), it changes to the Y-axis.

The force of gravity in one direction has an approximate reading of 1G.
Resting state is the negative value of Ready state. 
The axis is "opposite" when the club handle is pointing down and in Resting state, versus when the handle is upright and the sensor is in Ready state.

Some combination of coordinates might need to be calculated with advanced math for _gravity=zero._ But at this stage of development it probably isn't necessary.

**Physical Set-up:**

These readings will determine the Ready or Resting state orientation
- For ease of use, put the Arduino into a breadboard and then attach it to a stick in a perpendicular fashion as shown here. 
- _Imagine your golf club is either being used, or is sitting in the golf bag._

**Images: (1)Attach the device to a stick (2)Ready orientation (3)Device orientation (4)Resting orientation**

(1)<img src="/images/Sm-attaching to a stick.png" width="20%">
(2)<img src="/images/Sm-start orientation.png" width="20%">
(3)<img src="/images/Sm-device orientation.png" width="20%">
(4)<img src="/images/Sm-rest orientation.png" width="20%">

**The readings of the accelerometer, according to the setup in the images:**

There is a difference between the Ready(2) position and the Resting(4) orientation. When the device is attached as shown(1), one parameter, the Y-axis (in red) of the Accelerometer, tells the system whether it's in Ready state or it's in Resting state. When the Y-axis reading is positive then the sensor is in one state, and when it's negative it's in the other. Attached this way, the graph(3) shows that the Ready state is positive and the Resting state is negative.

**Notes about the Resting state:**

First, understand the orientation of the device. If the sensor finds that the club is sitting in the golf bag, then just one parameter is used from the accelerometer: `y < -.85`, meaning Resting state. It then checks every two seconds for a state change. The device is dormant in its Resting state and is not using more energy on sensors, and is not transmitting through BLE. 

The Resting state is meant for when the club is in the bag. If it's in the bag then it's not going to take readings. That would be wasteful. So it's meant to pause all the readings before any more readings are taken. The sensor will stay in Resting state until it senses Ready state. Once in Ready state, the sensors are turned back on again.

**What happens:**

1. powered on (rechargeable Lithium battery)
2. in the bag, so Resting state
3. pulled out of the bag, senses Ready state
4. at this point, **waits to settle** so it can begin recording motion [(in the next (gyro) section)](#collecting-gyro-data) _("waits to settle" is described later)_

**As shown in the serial monitor:**

- The Resting state:
  - the y-axis reading is near -1G (_-0.85_) and displays "One second delay..." 
  - then checks every 1 second using `delay(1000)`
- When `y > -.85`, then the Monitor shows "Ready!" 
  - and displays all the sensor readings (currently just acc)

<img src="/images/one second delay.PNG" width="50%"/>

_(This graphic is an early version of the other graphic where threshold have been adjusted to compensate for false state changes.)_

[ Top ](#golf-swing-sensors-overview)[| Accelerometer ](#sensor-accelerometer)[| Power ](#solving-for-power)[| Gyro ](#collecting-gyro-data)[]()[]()[]()








_(fine tune the following)_



## Simple Accelerometer Sketch

**Updating the Arduino Nano 33 BLE**

- **Find** the _SimpleAccelerometer_ sketch from the Example files in the **Arduino_LSM9DS1 folder**
- IMPORTANT: **Save it as** _golf-swing-acc_
- **Add the _if-else_ statements within the _void() loop_ as shown.** (the rest is unchanged) 
  - The _if/else_ statement creates the _-0.85_ threshold between the Ready and Resting states.

_**The following code is used to learn, and then built upon during the rest of this documentation:**_

_(verify that including `code` here is the best way to document)_

**Code for the new _LOOP_ is here:**
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

**Summary: Accelerometer Sensor**

This section was about setting up the Accelerometer, physically and with the IDE, so that it performs as expected.
The goal was to basically create on/off states, accomplished here by using a threshold for the Ready and Resting states. 

**Exception to this configuration:**

Swinging the club around won't put it into that Resting state unless it registers that particular state of inertia below _-0.85_. While there may be a risk of hitting that threshold while the club is in play, some cursory testing shows that it's possible the risk is low and `(y<-0.85)` doesn't happen or it doesn't hit the delay for some reason. _**The issue has been resolved in later code.**_ _(Must provide detail about this solution.)_

_(Maybe should compare both graphics with explanations to make a better point more succinctly.)_
- (put first graphic here)
- (explain that there needs to be an adjustment to avoid false state changes)
- (put second graphic next)
- (explain what happened, and maybe include the code that made it possible)

[ Top ](#golf-swing-sensors-overview)[| Accelerometer ](#sensor-accelerometer)[| Power ](#solving-for-power)[| Gyro ](#collecting-gyro-data)[]()[]()[]()









# Solving for Power

_Solving for power_ is a challenge to resolve during the [second physical stage](#physical-description) of development, when a second device is included, and the first needs independent power.

The solution described here will work for testing. But utilizing a new, more practical battery solution is required to collect **real** swing data.

**Current development power solution:**

- Connect **only** with BLE and be _physically detached_ from the computer.
- Attach the Arduino Sense (USBmicro port) to a power source.
- Use a lightweight phone recharger (with 2 USB out) to serve this purpose during development.  
- Rechargers will shut off after a short time with just a low power drain.
- _Charging an _old phone_ at the same time will prevent this auto-shutoff._ 

**Charging up a dead old phone, simultaneously powering Nano33BLE:**

<img src="/images/Sm-batterypack.png" width="35%">

**Battery options:**

- I am looking for those **2-prong** "magnetic" battery chargers, the kind of battery/connector is in that fit-watch, and where to get that rechargable battery.
- _(There is a small Lithium cell available, around 2-3mm. There is also needed a battery regulating circuit - but I don't remember what this is called - that's needed. There are three parts: The battery, the regulating circuit, and the connector. This connector is the "2-prong" connector I am referring to here.)_ 
- After researching and pinpointing what's needed, there will be a small lithium rechargable battery connected with wires to a "regulating circuit", with the wires attached to the desired connector, and the connector using USB on the other end.

**The round "2-Prong" connector:**

<img src="images/round connector.png" width="25%">

**Battery examples (that won't work):**

- **Qi coil** is a wireless charging device.
- **CR1220** is a small, common coin-type battery
- **LIR2032H** is a common 3.7 rechargable, but 20mm, so like a nickel size.

**9V option for [third stage](#physical-description) physical development:**

- There's a power solution in the TinyML Course, attaching a 9V battery to the **Learning Kit Shield**. 
  - This [**Appendix from the TinyMLx repo**](https://github.com/tinyMLx/appendix/blob/main/PoweringArduino.md#battery) is a good place to read about it.
- Not practical for the _golf-club-sensors_ prototype device itself, but is a good example for a stationary device.
- It's certainly not designed for swinging around, but it is proof that there's a pinout solution.

**Summary:**

We have enabled the Accelerometer, we have created a way with the accelerometer to determine whether the club is in the golf bag or being used. And we have found a temporary solution for power during development. 

Next, we need to try and collect data. We'll combine the sensors in some way, beginning with readings from the **gyroscope**.


_**Collecting data for each physical stage of development:**_
- **Stage 1: collect on computer, communicate directly**
- Stage 2: collect on computer, communicate via BLE
- Stage 3: collect on second device to SD card
  - MCU with attached SD card: nano33blesense or raspberry pi
- Stage 4: collect on tablet or smartphone

[ Top ](#golf-swing-sensors-overview)[| Accelerometer ](#sensor-accelerometer)[| Power ](#solving-for-power)[| Gyro ](#collecting-gyro-data)[]()[]()[]()




# Collecting Gyro Data

The orientation of the club head determines whether the club is in play or is sitting in the golf bag. The Ready/Resting state is determined by the readings from the **Accelerometer**. When in play, in Ready state, we're able to begin recording motion with the **Gyroscope**.

The code for the _Gyro_ will identify when the device is Still, and then and prepare to record movement. _(Is this where we put how the code looks when it's Still?)_


**Creating Data Points:**

Display the data as a set of coordinates plus millis to the Serial Monitor.
_**(Get started on the following in order to clarify all the rest of this page.)**_

**Building the code:**
- First, enable the Gyro to read and print all data points, (gX, gY, gZ) _**(Current Step)**_
- Change data points to INTEGERS _**(Current Step)**_
- Next, print data point plus millis, (gX, gY, gZ, gT) _**(Current Step)**_
  - Trying to figure out how to include millis now...
- Next, print data point only after each 100ms
- Next, set **threshold** to record data only when motion is faster. (When it's not _Still_.)
- Next, set millis to zero for first data point
- Print "No Movement Detected" one time, until movement is detected.
- Next, print data point plus millis (which begin at 0ms)
- **THIS is the set of data points to collect.**


**Description:**
- Swing data coordinates begin in one direction as a backswing.
- Then four seconds is recorded in 100ms increments
- Each increment saved as a data point
- Every data point saved into _an array of 100ms to 4000ms_
- Every array saved to a data set
- Collection of golf swings are the data set
- Real swing is appx four seconds
- All swings begin at 0ms
- All swings can be graphed in 3D space
- All swings can be shown in 3D motion


**Data Collection Steps:**

1. Display data points (coordinates plus millis)
2. Display array of data points
3. Display multiple arrays as a data set
4. Send data set to computer storage

Collect the series of points into an array.

- Collect data point **series** into an **array** in the code.
- Print array each time data point is added.
- Then, print data point series (the array) when motion stops

Collect arrays prior to output.

- Collect multiple arrays (swings)
- Accumulate 2 "swings" and print to Serial Monitor, purge memory 
- Accumulate 10 "swings" and print to Serial Monitor, purge memory 
- Create 4-second arrays
- Test accumulating 10, 25, 50, 100, 500 arrays before printing and purging

Send the data to the computer.

- Write the data to a file on the computer
- Append data to the CSV file


# Physical Development Stage Two

_Collecting data for each physical stage of development:_
- Stage 1: collect on computer, communicate directly
- **Stage 2: collect on computer, communicate via BLE**
- Stage 3: collect on second device to SD card
  - MCU with attached SD card: nano33blesense or raspberry pi
- Stage 4: collect on tablet or smartphone

**Stage Two adds the second device.** Configure the two devices to communicate via BLE.
The first device is powered independently and transmits the data to the second device.
The second device is attached to the computer, which operates the same way that it did before, because this is transparent to the computer. 
_The only thing that changes here is having BLE communication between two MCUs._








[ Top ](#golf-swing-sensors-overview)[| Accelerometer ](#sensor-accelerometer)[| Power ](#solving-for-power)[| Gyro ](#collecting-gyro-data)[]()[]()[]()

#
#

Collecting data for each physical stage of development:

- Stage 1: collect on computer, communicate directly
- Stage 2: collect on computer, communicate via BLE
- Stage 3: collect on second device to SD card
  - MCU with attached SD card: nano33blesense or raspberry pi
- Stage 4: collect on tablet or smartphone

#
#

_(Old notes:)_

_(Keep this statement.)_
Only in the **fourth stage** of physical development will we try and find a way to connect the device to a smartphone via BLE, which will allow a smartphone application to be built in the future.


_**Can I use the computer to collect the data?**_

- Data can only be sent to the Serial Monitor
  - Set of coordinates can be sent to the _Serial Monitor_.
  - Coordinates can be appended to an array and then sent to the _Serial Monitor_.
  - Multiple arrays can be appended to a variable and then later sent to _Serial Monitor_.
- Can this batch of arrays write to a CSV instead?
  - Device can send data via BLE.
  - There is no pairing for BLE, but there is driver for Win10 (assumes I want to use BLE on Windows, which I don't any more)
  - Can BLE be used to receive data from the device?
    - Configure data in first device, then send via BLE to second device
    - What form is that data in?
  - How to convert output to a CSV file? (collected on the SD card)
  - Can array be appended to a CSV? (simply append to the file)

_**Objectives:**_

- Collect multiple arrays into a CSV
- Collect consistant SWINGS data
- Graphically display array (coordinate series)
- Append more arrays to CSV
- 10X the data


_**Future considerations**_

- Explore how to enhance the swing data to include ball striking
- What new sensor measurements can this be adapted to include?





[ Top ](#golf-swing-sensors-overview)[| Accelerometer ](#sensor-accelerometer)[| Power ](#solving-for-power)[| Gyro ](#collecting-gyro-data)[]()[]()[]()



#
#

#### When to record data
We need to have a starting point for when to record data. 
Then we need to record 100 or 500 points of data, using physical movement. 
So what are the xyz coordinates when the Gyro printed out information? 
Record this data every 20ms to start with.
#
(I believe the following are the notes I wrote when developing the steps.)
#### How to record data 
**Where does this data go? Can it be stored within the Arduino Nano 33 BLE Sense?** 
_(This process is worked out at the beginning while configuring the gyro to print out data points. Eventually a step is to write to a CSV file. This takes place before adding the second device.)_
And then how to access it? What data can we collect? _(These questions have been answered by going through the steps creating the data points.)_
Once we collect data, can we spit it out to the Serial Monitor? _(This is the part about creating an array which spits to the Monitor when it's finished in 4secs.)_
Can we collect multiple instances of the swing? _(This is the part where multiple arrays are stored on the device and when the buffer is full it spits it out to the second device.)_

##
_(The following is repeated information, it's how I came up with the steps.)_
- Display Gyro data on Serial Monitor
  - _What does it represent?_
  - Use integers for gX, gY, gZ
- Set threshold to minimum movement before displaying data
- Add TIME STAMP, gT, using `millis()`
  - Set ms0 to first moment of movement
- Prepare data for collecting


##
_(I believe that the following has been distilled into the steps created and written above.)_
## How to collect a series of data points
- Find moment in accelerometer where, even though it's in Ready State, it is "still" enough to register to begin recording data
- Once "still", start recording data points, every 100ms, for 4 seconds. (This is 10 points of data per second, 4 seconds, which is 40 data points, and each data point is \[x,y,z,t], where t is the time stamp in ms from beginning of measurement.)
- Still State is priority over recording. If during recording the readings are again at Still, then reset and wait for movement again.
- From Still State goes backswing direction, followed by x,y,z,t as compared to its previous "location"
- Data point 0 is t=0ms. Data point 1 is t=100ms and has moved X distance in this direction from point 0. So, x1 - x0 = x for this data point.
- So one data point (p1) gets stored, and the next data point (p2) is in relation to prior data point. Data point p2 contains x,y,z, and a unique t compared to the other data points. One data record contains 160 bytes (_is this right?_) of data, which is x,y,z,t times 4 seconds, times 10 data points per second.
- Swing is measured in 5000ms, or 5 seconds. Once it ends, wait for Still State.

#
_(The following might need to be added above because it's asking how to identify **Still** state)_
#
**How to identify Still State**
- Device is already in Ready State
- Device is moving around measurably - as if waving in the air
- Device stops moving (within threshold of being still) - as if stationary upon a surface.
- Next, Device waits to discover movement and the first data point is recorded at x0,y0,z0,t00ms
- Next, second data point is recorded at x1,y1,z1,t100ms
- This continues for five seconds.

**Trim Data**
- Every swing begins with Still State followed by a backswing, swing, and then in 5 seconds stops recording.
- If Device detects motion becoming much slower on average, trim data here. (How to calc average? Distance between two data points is shorter than prior pair of data points on average.)  

**Trimmed Data has a 3D shape and speed** _(This is good, it considers what to do WITH the data set.)_
- Every swing can be layered upon another in different colors for example, and will create "normal" swing
- Outliers will be those which have almost no acceleration, the distance between data points is very small. (Nobody waves a club around as much as a practice swing or a real swing.)
- Difference between a Practice Swing and a Real Swing is the hitting of a ball, which will spike the accelerometer at around the fastest point of the swing.

**How to record sample data** _(This is good because it's talking about Still State, and might be helpful towards recording the data.)_
- On a table, Device is stationary
- Wave the device back and forth and then stop
- 40 data points should print out (per second), and no other information printed (Serial Monitor)
- Print swing to Serial Monitor, don't print non-swing
- For this example, when Device moves, print the information.
- But if it stops prior to 4 seconds, then it's Still State again, so don't print, will have fewer data points.
- Serial Monitor should print nothing except for a completed record of 4 seconds of movement.
- Print occurs when Still State is reached
- Motion, then Still State, then print data points of that Motion




**Steps** _(This is more notes about efforts to collect data. But also it's about BLE.)_
- First, get serial monitor going and get it to print out data points
- It can print out data points / plot, but can it collect them and print out **after movement stops?**
- It's possible to send "every sesson" of movement via BLE, if I can figure out how to RECORD the bytes, which are the x,y,z,t coordinates _(This is a BLE issue, so might not need to be here)_
  - And once it has been sent via BLE to a computer, can be compiled into a data set _(BLE sends data, CSV is created, then CSV is used in ML model.)_















#
#

_(Following are the useful statements and graphics previously written in the BLE section.)_\

#

[ Top ](#golf-swing-sensors-overview)[| Accelerometer ](#sensor-accelerometer)[| Power ](#solving-for-power)[| Gyro ](#collecting-gyro-data)[]()[]()[]()

_(Might be useful graphic?)_

(3) tilting on the y-axis to turn on/off the LED**

<p align="center">
  (3) <img src="/images/myBLEtilt.gif" width="30%">
</p>


_(Useful graphic here)_

<img src="/images/stateshanges.gif" width="80%">


_(Useful statements here)_
- Eliminate accidental state changes from the sensor
- Although things are working well, there are still two things we should improve upon. 
- One tweak is to accommodate for an unintentional state change from a bounce of the sensor, and the other is to reduce the amount of BLE communication, sending only once at the moment of a state change. _(re-write this para.)_
- We started with physically setting up the Arduino Nano33BLESense as if it were attached to the back of a golf club head.
- Then we implemented the code to be able to see the readings of the Accelerometer in the Serial Monitor screen.
- Reading the Accelerometer is important because it determines whether the golf club is actually being used (is in Ready state).

And BLE will need to be used to send swing data to a smartphone, a computer, or another MCU, because there's only 256k available within the Device.


[ Top ](#golf-swing-sensors-overview)[| Accelerometer ](#sensor-accelerometer)[| Power ](#solving-for-power)[| Gyro ](#collecting-gyro-data)[]()[]()[]()

#
#


