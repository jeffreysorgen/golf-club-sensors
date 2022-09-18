# Appendix

[Top ](https://github.com/jeffreysorgen/golf-club-sensors/tree/revised-README-1#golf-swing-sensors-overview)
[| Appx ](#appendix)[| BLE ](#enabling-ble)[]()[]()[]()[]()


_(The **Appendix** can contain code snippets as well, using a "back" link to refer to its source location.)_


# Enabling BLE
(Second Stage of Physical Development)

- _(There are some parts in this section which are specifically BLE and some that are NOT. These need to be separated, and anything not BLE related should be extracted and moved.)_

**Description:**

_Second Stage of Physical Development_

Before we can get it to chirp in response to a good or bad swing, the smartphone (because we're assuming physical _stage four_ of development) needs to pair up with the Arduino BLE Sense. We should be able to see on my Android whatever information we've already sent to the serial monitor. To do this, Nordic has an app we can download from Google Play called _nRF Connect_. Let's go through the process of getting that started right here. (BLE learning taking place here.)




**The BLE example sketch:**

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

**Caveat:**

With the USB cable plugged into the computer I **_can_** discover _"Arduino Environment Sensor"_ in nRF Connect. 
But a [**battery-only**](https://github.com/jeffreysorgen/golf-club-sensors/blob/revised-README-1/README.md#solving-for-power) solution can **_not_**. So...

**For battery-only:**

- Comment out `//while (!Serial);` 

After being untethered from the computer, the device was trying to find the serial port from which it's now disconnected.
So this one change will allow the device to function in _nRF Connect_ the same way as it did before.

**The Hello World BLE Sketch:**

Now that we've got the BLE connecting, and IMU data showing up in _nRF Connect_, it's time to simplify and specialize our code.

There is a _BLE Hello World_ sketch from [okdo.com](reference.md/#reference) 
that turns on the amber LED on the Arduino board when it connects, 
and we can then read "Hello World" on the Client (smartphone) app.
So starting with this simple code as a base, **we'll combine it with our own _golf-swing-acc_ sketch** 
so that we can see what gets sent from the Nano33BLESense.

- View the _Hello World BLE code_ [**here**](#ble-hello-world-code-is-here) --> _(need new link)_

The big takeaway with this code is that the `while (central.connected())` command just hangs the activity until BLE disconnects and then the code turns the LED off. 
(It's LED-centric code.) 
The LED will be useful for indicating "Ready" and "Resting" states.

_(The below section, Arduino File Structure, should be in the Appendix instead.)_

## Arduino File Structure

We've seen in those two examples what a basic `.ino` file looks like. **Here's a summary:**

Here _(**or maybe in an appendix instead**)_ we will describe the very basic structure of an Arduino `.ino` file. 

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
  - Give the Services and Characteristics their UUIDs ([here](reference.md/#uuid-info) for more info)
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

_(The above section "Arduino File Structure" is a stand-alone section in the (new) Appendix.)_

## Creating the new code

**Importing new functionality into our code:**

We're now going to take what we've learned from our two examples and incorporate them into our feature code, now called **_golf-swing-acc-ble_**.

- View the combined code [**here**](#all-the-golf-swing-acc-ble-code-is-here) <-- _(need new link)_

**Phone screen with device listed: (1) scanning, (2) connected, (3) tilting on the y-axis to turn on/off the LED**

<p align="center">
  (1) <img src="/images/BLEScanning.png" width="20%">
  (2) <img src="/images/BLEConnected.png" width="20%">
  (3) <img src="/images/myBLEtilt.gif" width="30%">
</p>

**Almost done**

_(This para is sensor-related, so it should be extracted and kept.)_
Although things are working well, there are still two things we should improve upon. 
One tweak is to accommodate for an unintentional state change from a bounce of the sensor, and the other is to reduce the amount of BLE communication, sending only once at the moment of a state change.
 

So we created **_golf-swing-acc-ble-statechange_** with this modified code: 
- Eliminate accidental state changes from the sensor
- Send data via BLE **only** when the state changes _(This means that BLE communication begins when removed from Resting state.)_
- Send _boolean 1/0_ rather than the strings, "Resting" and "Ready" _(This note is specifically about bytes transmitted, rather than 5-7 chars.)_
- Use the shorter 16-bit UUID, like `ffe0` and `ffe1` [_(More about UUID)_](reference.md#uuid-info)

_(The following fact should be referenced, but "State Change" needs to be extracted and kept in the Sensors documentation rather than just the BLE documentation. It has not yet been determined how much and when BLE transmits; first finish physical **stage one**.)_

**BLE sends data only when the words "State change to" appear:**

<img src="/images/stateshanges.gif" width="80%">

**Summary: Enabling BLE**

**Server/peripheral-side BLE programming is done for now.**
We started with physically setting up the Arduino Nano33BLESense as if it were attached to the back of a golf club head.
Then we implemented the code to be able to see the readings of the Accelerometer in the Serial Monitor screen.
After experimenting with a couple of example sketches, we incorporated the BLE library into the code, downloaded the nRF Connect application to a smartphone, and were **able to see readings** coming in. 

The Nano33BLESense has now been programmed to communicate with a Client (central), 
so it's time to develop an Android application that it can control, basically with an on/off signal sent through Bluetooth Low Energy. 
(Can't do this myself. Building an app will require a large investment. $$)
**Enabling BLE communication with a smartphone will instead be shifted out to a step after Data Collection.** (Which means it's after physical stage one of development.)
_Rather than focusing immediately on the smartphone application, we should collect data on the device which can then be sent via BLE. (Meaning, after stage one development.)_

**Two Important Points:**

This "Enabling BLE" section above describes two things: One, reading data from the Accelerometer, and two, setting up one side of a BLE connection.
Reading the Accelerometer is important because it determines whether the golf club is actually being used (is in Ready state).
And BLE will need to be used to send swing data to a smartphone, a computer, or another MCU, because there's only 256k available within the Device.

_(BLE is used in physical development [**stage two**](#physical-description). The above section, "Enabling BLE", needs to get bumped to **after** physical stage one development has been **completed**. This means that first the system must be fully functional without BLE.)_



