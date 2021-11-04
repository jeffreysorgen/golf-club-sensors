[*[ Overview ]*](README.md/#golf-swing-sensors)
[*[ 1 The Accelerometer ]*](implementation.md/#the-accelerometer)
[*[ 2 Solve for Power ]*](implementation.md/#solve-for-power)
[*[ **Step Three: Enable BLE+IMU** ]*](#step-three)
[*[ Step Four: Enable Smartphone Response ]*](activity.md#step-four)
[_[ jump to new project ideas-> ]_](thoughtsandnotes.md/#other-projects)

##
##### Step Three:
# Enabling BLE

**Description:**

Before we can get it to chirp in response to a good or bad swing, the smartphone needs to pair up with the Arduino BLE Sense. We should be able to see on my Android whatever information we've already sent to the serial monitor. To do this, Nordic has an app we can download from Google Play called _nRF Connect_. Let's go through the process of getting that started right here. 

### Try the BLE example sketch
I found -[_**this video**_](https://youtu.be/2q_tA8v5l1Y)- from _Robocraze_ to be helpful, 
and copied the code from its [**accompanying GitHub repository**](https://github.com/Robocraze/Nano-33-BLE-Examples/blob/43fbe5b3155493d3056e85d7402c54e05c84f133/environment_sensor_ble/environment_sensor_ble.ino).
This example reads information from the sensors and then simply displays it in the phone app. 
Upload the sketch to the device... _**And it works exactly as it does in that video.**_ 
_But there is a [**caveat**](#caveat) which I discovered for this example._ 
We'll refer to this _RoboCraze_ code for reference in the future if we need to.

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
9. (App) Touch the "triple down arrow" <img src="images/3downarrows.png" width="20em" /> for each of the three services for this example
10. (App) OBSERVE the temperature gradually reach ambient room temperature or hold in hand for it to rise

##### Caveat:
With the USB cable plugged into the computer I **_can_** discover _"Arduino Environment Sensor"_ in nRF Connect. 
But a [**battery-only**](implementation.md/#current-development-solution) solution can **_not_**. So...

**For battery-only:**

- **Comment out `//while (!Serial);`** 

After being untethered from the computer, the device was trying to find the serial port from which it's now disconnected.
So this one change will allow the device to function in nRF Connect the same way as it did before.

##
### The Hello World BLE Sketch

Now that we've got the BLE connecting, and IMU data showing up in nRF Connect, it's time to simplify and specialize our code.

There is a simple _BLE Hello World_ sketch from [okdo.com](#reference) that turns on the amber LED on the Arduino board when it connects. And after connecting with nRF Connect, we can read "Hello World" on our smartphone.

_What's interesting to me (newbie!) is using `static const char* greeting = "Hello World!";` first, and then later using `greetingCharacteristic.setValue(greeting);` for that string to appear through the characteristic._

##

Starting with this simple code as a base, **we'll combine it with our own _golf-swing-acc_ sketch** 
so that we can see what gets sent to _nRF Connect_ from the BLESense.

##### _BLE Hello World_ code is here:
```
/*
  Arduino Nano 33 BLE Getting Started
  BLE peripheral with a simple Hello World greeting service that can be viewed
  on a mobile phone
  Adapted from Arduino BatteryMonitor example
*/

#include <ArduinoBLE.h>

static const char* greeting = "Hello World!";

BLEService greetingService("180C");  // User defined service

BLEStringCharacteristic greetingCharacteristic("2A56",  // standard 16-bit characteristic UUID
    BLERead, 13); // remote clients will only be able to read this

void setup() {
  Serial.begin(9600);    // initialize serial communication
  while (!Serial);

  pinMode(LED_BUILTIN, OUTPUT); // initialize the built-in LED pin

  if (!BLE.begin()) {   // initialize BLE
    Serial.println("starting BLE failed!");
    while (1);
  }

  BLE.setLocalName("Nano33BLE");  // Set name for connection
  BLE.setAdvertisedService(greetingService); // Advertise service
  greetingService.addCharacteristic(greetingCharacteristic); // Add characteristic to service
  BLE.addService(greetingService); // Add service
  greetingCharacteristic.setValue(greeting); // Set greeting string

  BLE.advertise();  // Start advertising
  Serial.print("Peripheral device MAC: ");
  Serial.println(BLE.address());
  Serial.println("Waiting for connections...");
}

void loop() {
  BLEDevice central = BLE.central();  // Wait for a BLE central to connect

  // if a central is connected to the peripheral:
  if (central) {
    Serial.print("Connected to central MAC: ");
    // print the central's BT address:
    Serial.println(central.address());
    // turn on the LED to indicate the connection:
    digitalWrite(LED_BUILTIN, HIGH);

    while (central.connected()){} // keep looping while connected
    
    // when the central disconnects, turn off the LED:
    digitalWrite(LED_BUILTIN, LOW);
    Serial.print("Disconnected from central MAC: ");
    Serial.println(central.address());
  }
}
```

**How we will use this:**

The big takeaway with this code is that the `while (central.connected()){` command just hangs the activity until BLE disconnects and then the code turns the LED off. 
**_It's LED-centric code._** 
The code for the LED will be useful for indicating "Ready" and "Resting" states.

**_We've seen in those two examples what a basic `.ino` file looks like._** Here's a summary...

## Arduino File Structure

Here we will describe the very basic structure of an Arduino `.ino` file. 

**At the most basic level, there are four sections:**

1. *"prior to"*
2. `void setup()`
3. `void loop()` and
4. *"other functions"*

##

**1. prior to `void setup()`**

- These can be within _namespace_
- First add LIBRARIES
- Initialize and name CONSTANTS
- Initialize VARIABLES
- Initialize BLE SERVICES
  - Give the Services and Characteristics their UUIDs ([here](#uuid-info) for more info)
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


## Creating the new code

**Importing new functionality into our code:**

We're now going to take what we've learned from our two examples and incorporate them into our feature code, now called **_golf-swing-acc-ble_**.

##### All the _golf-swing-acc-ble_ code is here:
```
/*
 * Arduino LSM9DS1 
 * - Simple Accelerometer
 * golf-swing-acc
 * golf-swing-acc-ble
 */
 
// LIBRARIES
#include <ArduinoBLE.h>         // BLE library
#include <Arduino_LSM9DS1.h>    // IMU library

// CONSTANTS
static const char* imuUUID = "355d2b52-982c-4598-b9b4-c19156686e1a";
static const char* accUUID = "9e5982a7-9ef0-48e0-a167-8112ada5f184";
static const char* stateUUID = "9dc52af2-d585-4fb7-93a7-922b463239fe";

// INITIALIZE VARIABLES
/*
 * (example) String p, t, m; // Initalizing global variables for...
 */

// BLE SERVICE NAME (create service)
BLEService IMUService(imuUUID);

// BLE CHARACTERISTICS (create characteristics)
BLEFloatCharacteristic ble_accelerometer(accUUID, BLERead | BLENotify); // for the IMU service
BLEStringCharacteristic ble_state(stateUUID, BLERead | BLENotify, 10); // for the IMU service

// FUNCTION PROTOTYPE
/*
 * set up more functions here
 */

void setup() {
  // INITIALIZE THE SENSORS (and serial)
  IMU.begin();          // initialize IMU
  Serial.begin(9600);   // initialize serial comms
  //while (!Serial);    // comment this out

  // INITIALIZE THE DEVICE PINS
  pinMode(LED_BUILTIN, OUTPUT); // initialize the built-in LED pin
  
  // CHECK FOR FAILURE
  // BLE check
  if (!BLE.begin()) {
    Serial.println("starting BLE failed!");
    while (1); 
  }
  // IMU check
  if (!IMU.begin()) {
    Serial.println("Failed to initialize IMU!");
    while (1); 
  }
  
  // SET BLE NAME (create device name)
  BLE.setLocalName("Nano33BLESense");
  
  // ADVERTISE SERVICES
  BLE.setAdvertisedService(IMUService);
  
  // ADD CHARACTERISTICS TO BLE SERVICES
  IMUService.addCharacteristic(ble_accelerometer);
  IMUService.addCharacteristic(ble_state);
  
  // ADD SERVICES TO BLE STACK (add service to advertise)
  BLE.addService(IMUService);    // Add IMU Service
  
  // SET VALUES FOR STRINGS
  /*
   * setValue(x) stuff here
   */
  
  // START ADVERTISING (advertise all services)
  BLE.advertise();
  
} //s

void loop() {
  // Wait for a BLE central to connect
  BLEDevice central = BLE.central();

  // IMU activity
  float x, y, z;

  if (IMU.accelerationAvailable()) {
    IMU.readAcceleration(x, y, z);

    ble_accelerometer.writeValue(y);    // sends hex value

    // threshold is -1G
    if ( y > -.85 ) {
      
      // print to BLE
      ble_state.writeValue("Ready!");
      
      // engage the LED
      digitalWrite(LED_BUILTIN, HIGH);
      
      // print to Serial port
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
      
      // print to BLE
      ble_state.writeValue("Resting!");
      
      // engage the LED
      digitalWrite(LED_BUILTIN, LOW);
      
      // print to Serial port
      Serial.print("One second delay...");
      Serial.print('\t');
      Serial.print("Y = ");
      Serial.println(y);
      delay(1000); // one second delay
      }
    }
  } //v
```

##

##### Phone screen with device listed: (1)Scanning, (2)Connected, (3)Tilting on the y-axis to turn on/off the LED
<p align="center">
  (1)<img src="images/BLEScanning.png" width="20%">
  (2)<img src="images/BLEConnected.png" width="20%">
  (3)<img src="images/myBLEtilt.gif" width="45%">
</p>

##
## Summary so far:

**We have created code that displays Ready/Resting on the serial monitor and in the nRF Connect app, but also, we got the LED on the device to light up as well!**

In this section we transformed the _golf-swing-acc_ code to include BLE communication. We took two example sketches to learn about the Arduino file structure and imported the code we needed into **_golf-swing-acc-ble_** to enable it to communicate with a smartphone running the _nRF Connect_ app.

1. Data is being sent from the device to nRF Connect
2. The device sends "Ready"/"Resting" to the app depending on a threshold in the code
3. Accelerometer value is sent in the form of a **_hex_** (don't know if this makes a difference to the project)
4. We used UUIDs in their long form as constants [_(Notes about UUID)_](activity.md#uuid-info)
5. Then started to use a shorter, 16-bit form of UUID, like "ffe0" and "ffe1"

##

But we don't want information to be sent via BLE every time the code loops.
Instead, we need to send notifications about a change of state to the Client (nRF Connect) when the sensor changes its state from Ready to Resting or back.
When the Client app reads "Ready!" or "Resting!" it receives every character (6 or 8 bytes of information) from the device constantly, which is excessive.
_(It's sending all y-axis information right now too, but that will change later)_


The code:
- Add logic to the code so that Resting is 1 and Ready is 0
- Add an interval buffer to accommodate for any unintentional bounces when it checks its state.
- Write to BLE only when the state changes

............

We modified _golf-swing-acc-ble_ (now **_golf-swing-acc-ble-statechange_**) to include the code that will notify the client of a state change.

We can see this change in the Value field of the UUID ("0xFFE2") for the specific characteristic: `value (0x) 00` and `value (0x) 01`.

............

**Now that state changes can be sent to the smartphone, try to turn its flashlight on/off with the signal!**
What we want to do for this project is to read information from the sensor and then get the phone app to act upon the capabilities of the phone, such as turning on a flashight or beeping. 
While the flashlight functionality won't be used in the end, that solution is crucial for when we're trying to get the phone to chirp good/bad golf swings. 
- There is a difference between constantly notifying about the state and simply notifying about a state change.
- Notifying only about a state change will be helpful to eliminate unnecessary BLE communication. 
- Checking a state change can happen less frequently than the device baud rate, so we don't get bounces of the states due to natural movement. 
  - For example, during its transition to a new state the LED lit very briefly, flashing the previous state of the LED. It looked like a bounce.

.............

Flashlight:
- When the state goes from 0 to 1, I want the phone's flashlight to turn on. When it goes from 1 to 0, should turn off.
- More directly, state change into and out of Ready/Resting states. If `y < -.85` then turn on the flashlight on my phone!

.............

**Important to enable Notify for Change of State**

One of the future modifications needs to be utilizing the BLE code that features **state change only** notifications, so that nRF only receives one-time signal that the state has changed between Ready and Resting, rather than as it is now, which always prints its state to BLE. 

.............

- So we need to enable functionality within _nrfconnect_ to do something.
- We've already sent the data once, so do something, and when it changes, do something else.
- Code: When Y-axis, `y < -0.85`, changes from True to False or back, this is the moment to send BLE data and nothing else, to save on BLE energy.
- We're going to try to enable one element of the BLE functionality which will send a notification only when the state changes.
- The code will send a change of state notification when it happens, which can then be held in the nRF Connect app until the next update.

.............








............
##
Making that change, and...
### Peripheral-side code is done

**The new code now has this function.** (_golf-swing-acc-ble-statechange_) 
We modified the code so that it will only send data via BLE when necessary, when the state switches between Ready and Resting. 
This reduces the BLE communication (which is the most energy-hungry part of this project) down to a single instance: _a characteristic change_ (state change). 
We substituted using entire words, "Resting" and "Ready", and instead now use _boolean 1/0_ to do the same thing.

It sends BLE data only at the point of the state change (note the words "State change to" in the monitor)

##### State changes by tilting on the y-axis
  <p align="center"><img src="images/stateshanges.gif"  width="80%"></p>


#

Now:
- _Most likely, need to just figure out what to do with an Android app, and **act upon the UUID changing from 0 to 1**_

This is the research I am doing now.
- nRF Connect to respond by making my phone beep or flashlight on and off (or more likely, using Android Studio instead)

#

#

#

##
##### State change: (pseudo code)

This **_pseudo code_** models the code we used to transform into functionality that updates the current readyState every half-second. (used `millis()`) 
In the loop it checks whether the state has changed, and if it did, it sends boolean data to the client smartphone.

##### still need this pseudo code for reference but can delete later
```
resting = state("Resting");
ready = state("Ready");
earlier = now;
now = update.state();        // returns "Ready" or "Resting"

if ( now !== earlier ) {     // if state has now changed
    if (now == resting) {    // and is now Resting
        beep(low);           // then beep low for new Resting state
        notifyBit = 0;
        }
    else if (now == ready) {
        beep(high);          // otherwise beep high for new Ready state
        notifyBit = 1;
        }
    blenotify(notifyBit);    // sends BIT to BLE "descriptor" (or something)
    }

else {
    pass;                    // now == earlier, so no state change
    }

earlier=now;                 // update earlier state with now state

```

#

#

##### (reference)
**Notify or Indicate.** 
(Here's something from [ArduinoBLE Reference](https://www.arduino.cc/en/Reference/ArduinoBLE))
Think of this as _Sender_ and _Reader_. 
ArduinoBLESense device is the _sender_, or Peripheral. 
When a reading changes, the nRF Connect app is going to be the _reader_, or Client.
The model BLE uses is known as a "publish-and-subscribe" model.

#

**Stuff I'm no longer considering but might be useful:**
- There may be BLE-specific code that transmits _only_ when there's a state change, and could shorten this entirely, but for now I built it into this code. 
- Is there a way for the client to ask the peripheral whether the state has changed? Maybe. But how frequently and how much power consumption. Of course, the peripheral could ignore requests for update as well. Unless there's another way to think about this, I don't think this matters much. No savings of effort or energy.
- Another way to look at this is by doing a check on **whether the states match** on the peripheral and client, and if it doesn't, to update the client, although it might require more communication between devices.


#

**might be unrelated to our own project**

Here's the [FORUM for Arduino.](https://forum.arduino.cc/c/using-arduino/programming-questions/20)
- COPY HIS CODE

Go through entire process this user did implementing Notify/Indicate. **However this is developed, it's going to need CCCP, whatever that is.** 
- [(link to question) Set CCCD value](https://forum.arduino.cc/t/feature-request-option-to-set-cccd-value/919852)
- [(link to question) Notify/Indicate](https://forum.arduino.cc/t/notifications-and-indications-disabled-nrf-connect/915757)

He's discussing the nRF Connect functionality with Notify and Indicate. Also refers to video mentioned here about [BLE on Arduino](https://youtu.be/osneajf7Xkg) which shows some detail about Server/Client and characteristics
  - In this he mentions "BLE2902" _(figuring that out next)_ 
  - In nRF Connect shows up as "0x2902"

**Should go back to this FORUM to see if this makes more sense now that I finished with this step.**
#

#
