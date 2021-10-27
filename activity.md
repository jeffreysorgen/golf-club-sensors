- update facts about _golf-swing-hello-world_ 
  - (copy entire code here)
- add photo of battery/old phone arrangement

[*[ Overview ]*](README.md/#golf-swing-sensors)
[*[ The Accelerometer ]*](implementation.md/#the-accelerometer)
[*[ Solve for Power ]*](implementation.md/#solve-for-power)
[*[ **Step Three: Enable BLE+IMU** ]*](#step-three)
[*[ Step Four: Enable Smartphone to BEEP ]*](#step-four)
[_[ jump to new project ideas-> ]_](#jot-down-ideas-for-other-projects-here)

#
##### Step Three:
## Enabling BLE

#### Description
Before we can get it to chirp in response to a good or bad swing, the smartphone needs to pair up with the Arduino BLE Sense. We should be able to see on my Android whatever information we've already sent to the serial monitor. To do this, Nordic has an app that I downloaded from Google Play called _nRF Connect_. I'm going to go through the process of getting that started right here.

### Try the BLE example sketch
I found -[_**this video**_](https://youtu.be/2q_tA8v5l1Y)- from _Robocraze_ to be helpful, 
and copied the code from its [**accompanying GitHub repository**](https://github.com/Robocraze/Nano-33-BLE-Examples/blob/43fbe5b3155493d3056e85d7402c54e05c84f133/environment_sensor_ble/environment_sensor_ble.ino).
This example reads information from the sensors and then simply displays it in the phone app. Upload the sketch to the device... _**And it works exactly as it does in that video.**_ But there is a [*__caveat__*](#caveat) which I discovered for this example.
#### To do this:
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

#### For battery-only:

- **Comment out `//while (!Serial);`** 

After being untethered from the computer, the device was trying to find the serial port from which it's now disconnected.
So this one change will allow the device to function in nRF Connect the same way as it did before.

#
### The Hello World BLE Sketch

Now that we've got the BLE connecting, and IMU data showing up in nRF Connect, it's time to simplify and specialize our code. There is a simple _BLE Hello World_ sketch from [okdo.com](#reference) that turns on the amber LED on the Arduino board when it connects. 

Starting with this simple code as a base, we'll combine it with our own _golf-swing-acc_ sketch
so that we can see the accelerometer data inside the _nRF Connect_ app.
We'll refer to the _RoboCraze_ sketch for reference if needed.

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
##### note:
_(Maybe just move this code to an appendix. I think I only used on/off, connected/disconnected on the LED. Because that's the big takeaway from this exercise. But there was also a lesson with the `while` command. It just hung there until disconnected, and the LED turned off because of it. No further useful functionality.)_


#
## Structure of Arduino files

We will be combining code from the two example sketches with **the accelerometer sketch**
so we need to understand the structure of a very basic `.ino` file. 

##### (Entire code is posted elsewhere and these are examples)

At the most basic level, there are four sections:
1. *"prior to"*
2. `void setup()`
3. `void loop()` and
4. *"other functions"*

#### 1. **Prior to `void setup()`**
- These can be within _namespace_
- First add LIBRARIES
  - `#include <Arduino_LSM9DS1.h>  // IMU library`
  - `#include <ArduinoBLE.h>  // BLE library`
- Set CONSTANTS
  - `static const char* greeting = "Hello World!";`
  - `static const char* greetingUUID = "355d2b52-982c-4598-b9b4-c19156686e1a";`
- Initialize VARIABLES
  - _example:_`String p, t, m; // Initalizing global variables for...` (omit)
- Add SERVICES
  - Give the Services and Characteristics their UUIDs ([here](#uuid-info) for more info)
  - `BLEService customService("180C"); // means "user-defined, unregistered generic UUID"`
  - `BLEService greetingService(greetingUUID);`
- Add respective Service CHARACTERISTICS
  - `BLEStringCharacteristic ble_accelerometer("2A58", BLERead | BLENotify, 20);`
  - `BLEStringCharacteristic greetingCharacteristic("2A56", BLERead, 13);`
- Create the FUNCTION PROTOTYPE ("other functions")
  - _example:_`void readValues();`  

#### 2. `void setup()`
- INITIALIZE THE SENSORS
  - `IMU.begin(); // initialize the sensors`
- Initialize SERIAL COMMUNICATION
  - `Serial.begin(9600);`
  - `//while (!Serial);    // comment this out` This will hang if the computer is detached
- And initialize OTHER things
  - `pinMode(LED_BUILTIN, OUTPUT); // initialize the built-in LED pin` 
- Check for FAILURE
``` 
        if (!BLE.begin()) {
          Serial.println("starting BLE failed!");
          while (1);
        }
        if (!IMU.begin()) {
          Serial.println("Failed to initialize IMU!");
          while (1);
        }
```
- Set the NAME to show up in the SCAN
  - `BLE.setLocalName("Jeff's Nano33BLE");`
- Set BLE SERVICE ADVERTISEMENT
  - `BLE.setAdvertisedService(customService);`
- ADD CHARACTERISTICS to the BLE services
  - `customService.addCharacteristic(ble_accelerometer);`
- ADD SERVICE to the BLE stack
  - The variable names were previously declared in "Add Services" section prior to the setup loop
  - `BLE.addService(customService);  // Adding the service to the BLE stack`
  - `BLE.addService(greetingService); // Add Text service`
- Set VALUES for strings
  - This variable ("greeting") was set previously, in the "Constants" section prior to the setup loop
  - `greetingCharacteristic.setValue(greeting);  // Set greeting string; Set values`
- ADVERTISE
  - `BLE.advertise();  // Start advertising`
  
#### 3. `void loop()`
- `BLEDevice central = BLE.central(); // Wait for a BLE central to connect`
  - Waiting for central to connect might mean that other things can't go on first (so be aware)
- if statements:
```
  if (IMU.accelerationAvailable()) {
    IMU.readAcceleration(x, y, z);
```
- This part checks sensor reading for y-axis, prints "Ready" or "Resting" to monitor and to nRF Connect, and also turns on or off the LED
  - `if (central)` is likely not needed here, but helped me understand when and if BLE was connecting 
```
    if ( y > -.85 ) {
      Serial.println("Ready!");
      ble_accelerometer.writeValue("Ready!");
      if (central) { digitalWrite(LED_BUILTIN, HIGH);
        }}

    else {
      Serial.println("Resting!");
      ble_accelerometer.writeValue("Resting!");
      if (central) { digitalWrite(LED_BUILTIN, LOW);
        }}
  }
```
#### 4. **other functions**
- _example:_`void readValues() { // etc`


#





The result:
##### All the _golf-swing-hello-world_ code is here:
```
/*
 * Hello World from okdo.com
 * Adapted from Arduino BatteryMonitor example
 * golf-swing-acc
 * golf-swing-hello-world
 *
*/

// LIBRARIES
#include <ArduinoBLE.h>         // BLE library
#include <Arduino_LSM9DS1.h>    // IMU library

// CONSTANTS
static const char* greeting = "Hello World!";
static const char* greetingUUID = "355d2b52-982c-4598-b9b4-c19156686e1a";

// BLE SERVICE NAME
BLEService customService("180C");           // for the IMU service
BLEService greetingService(greetingUUID);   // for the Text service

// BLE CHARACTERISTICS
BLEStringCharacteristic greetingCharacteristic("2A56",  // standard 16-bit characteristic UUID
    BLERead, 13); // remote clients will only be able to read this
BLEStringCharacteristic ble_accelerometer("2A58", BLERead | BLENotify, 20);
    // "2A58" is arbitrary

// FUNCTION PROTOTYPE

void setup() {
  // INITIALIZE THE SENSORS (and serial)
  IMU.begin();          // initialize IMU
  Serial.begin(9600);    // initialize serial communication
  //while (!Serial);    // comment this out

  // INITIALIZE THE DEVICE PINS
  pinMode(LED_BUILTIN, OUTPUT); // initialize the built-in LED pin

  // CHECK FOR FAILURE
  if (!BLE.begin()) {   // initialize BLE
    Serial.println("starting BLE failed!");
    while (1);
  }
  if (!IMU.begin()) {
    Serial.println("Failed to initialize IMU!");
    while (1);
  }

  // SET BLE NAME
  BLE.setLocalName("Jeff's Nano33BLE");  // Set name for connection

  // ADVERTISE SERVICES
  BLE.setAdvertisedService(customService);    // 'customService' is for IMU
  BLE.setAdvertisedService(greetingService); // Advertise service

  // ADD CHARACTERISTICS TO BLE SERVICES
  customService.addCharacteristic(ble_accelerometer);
  greetingService.addCharacteristic(greetingCharacteristic); // Add characteristic to service

  // ADD SERVICES TO BLE STACK
  BLE.addService(customService);    // Add IMU Service
  BLE.addService(greetingService); // Add Text service
  
  // SET VALUES FOR STRINGS
  greetingCharacteristic.setValue(greeting); // Set greeting string

  // START ADVERTISING
  BLE.advertise();  // Start advertising

  // extra printing stuff
  Serial.print("Peripheral device MAC: ");
  Serial.println(BLE.address());
  Serial.println("Waiting for connections...");
}

void loop() {
  BLEDevice central = BLE.central();  // Wait for a BLE central to connect
  float x, y, z; // from golf-swing-acc sketch

  if (IMU.accelerationAvailable()) {
    IMU.readAcceleration(x, y, z);
    
    if ( y > -.85 ) {
      Serial.println("Ready!");
      ble_accelerometer.writeValue("Ready!");
      if (central) { digitalWrite(LED_BUILTIN, HIGH);
        }}

    else {
      Serial.println("Resting!");
      ble_accelerometer.writeValue("Resting!");
      if (central) { digitalWrite(LED_BUILTIN, LOW);
        }}
  }
} //v
```




#
#
#
#



##### Digging into App Dev

As it turns out, we didn't see the accelerometer data inside the app, but we were able to pass text into the app, such as "Ready" and "Resting".
I don't know whether it will be necessary to transfer this kind of raw data to the app.
"Ready" and "Resting" could be read in the app as hex and as ASCII text. (one byte per letter)

In the code, we need to send data of a type that can be meaningful to nRF Connect. 
I will need to understand how to program my phone with it now that I'm getting data.
I need to learn how nRF Connect interfaces with my Android.

**And this brings us to developing with nRF Connect!**

### For nRF Connect Development:
- I need nRF Connect for Desktop:
[link](https://www.nordicsemi.com/Products/Development-tools/nRF-Connect-for-desktop/Download?lang=en#infotabs)
- There is a nRF Connect for VS Code, downloadable from the Toolchain Manager in nRF Connect for Desktop:
[link](https://www.nordicsemi.com/Products/Development-tools/nRF-Connect-for-VS-Code/Download#infotabs)
- There are videos for installation:
[YouTube](https://youtu.be/2cv_jjqk5hg)






#
[_[ Link to **Structure of Arduino Files** ]_](#structure-of-arduino-files)
#
#
#
#


# Notes about combining the files:

##### Combine code from hello world and golf-swing-acc
1. Open golf-swing-acc (done)
2. Open Hello World (done)
3. Save Hello World as _golf-swing-hello-world_ (done)
4. Add the IMU stuff to _golf-swing-hello-world_ (done)
5. Delete extraneous code from new sketch (done, saved, version step is final)

#

(doing this)
- Take the BLE commands and integrate them into my _golf-swing-acc_ sketch: (done)
- Try **reversing** the action, and take my IMU sketch and pull it line by line **into** the _Hello World_ sketch. (did this, delete)
  - For one thing, I can read what was sent from the device, also, I confirmed the non-serial battery solution applied. (test battery-only on 'golf swing hello world')
- Save _golf-swing-acc_ as _testing-ready-resting-imu-ble_ (done) (rewrite this to fact) (n/a - we're at a point where the only file I want is the 'golf swing hello world' one, so determine what I used and what is extraneous, and drop extra in delete folder)
- Add in code for BLE as appropriate (done)

#

- My [integrated IMU/BLE configuration](#modifying-the-file) displayed a **hex value** in _nRF Connect_ rather than readable data.
  - Need to transform the hex value into a readable one. (Explore more BLE examples first.) (might be next phase, maybe could ignore)
  - I am able to get strings go go through, just not floats. Don't know if this is relevant.
- Make the nRF Connect readings display "Ready" / "Resting" **(DONE!)**
  - **I have created a file that shows Ready/Resting on the serial monitor and on the nRF Connect app, but also, I have gotten the LED on the device to light up as well!**
  - Once that question is answered, I'll get my phone to turn its flashlight on/off as a result. (yep)












#
##### Link to entire file here: [**_golf-swing-hello-world code_**](#all-the-golf-swing-hello-world-code-is-here)

#
#





# UUID Info:

#### Notes about UUID

- At this point, figure out the **UUID** information.
- Need to have specific UUIDs for each IMU param
- I can't find a specific UUID for x,y,z on the Accelerometer
  - what does the **raw** IMU data look like?
  - check the hackster.io post in the [Reference](#reference)
- I thought I would need to use the BLE UUID spec which I thought was "important for ble" [(below)](#reference)
- **Keep studying about UUID**
- Go simpler. Look where they use them in Hello World, for example, and use theirs instead. It's not like we're getting in trouble or having technical conflicts with them.
- Also go back to the [hackster](https://www.hackster.io/gov/imu-to-you-ae53e1) site again

#### (15 unique v4UUIDs)
```
355d2b52-982c-4598-b9b4-c19156686e1a
9e5982a7-9ef0-48e0-a167-8112ada5f184
9dc52af2-d585-4fb7-93a7-922b463239fe
8564aabe-417c-4fe4-8a40-543ea08079f4
3e8c97c5-6ae5-444f-b56e-20a741e7bf99
f2024cef-dae8-4db7-bddb-76c696cdc115
62237f9d-7652-442a-a36a-0a68d96bd617
f22e0e5c-9636-4a83-9eaa-ba309101c4b6
88abdb86-22fb-4b2f-8d35-53bb942625b7
0a374697-2847-4d15-b7b2-b89281022f65
b60136d1-e8c5-4042-82e8-a0f6fcd4f6d4
989ae3c5-6e5a-4868-96c7-011511e880b2
d49b4462-2bfd-4d92-8103-88ed9429e662
52f6c067-db43-4e53-893b-d7d98406901b
fa94204d-dc71-4585-aa63-98b8133c5266
```

#

#

#

#






(good notes here)
##### Description:
What we want to do for this project is to read information from the sensor and then 
get the phone app to act upon the capabilities of the phone, such as turning on a flashight or beeping. 
While the flashlight functionality won't be used in the end, that solution is crucial for when we're trying to 
get the phone to chirp good/bad golf swings. 
- There is a difference between constantly notifying about the state and simply notifying about a state change.
- Notifying only about a state change will be helpful to eliminate unnecessary BLE communication.
- Checking a state change can happen less frequently than the device baud rate, so we don't get bounces of the states due to natural movement. 
  - Best example was when the LED lit briefly, flashing the previous state of the LED during the transition to a new state. It looked like a bounce.

#
#
#
#

# Reference:

- Here's the okdo.com example, including [_BLE Hello World_](#the-ble-hello-world-sketch): [**getting started** from *okdo.com*](https://www.okdo.com/getting-started/get-started-with-arduino-nano-33-ble/#h-1-configure-ide-toc)
- XXXXXXXX-0000-1000-8000-00805F9B34FB Look this up to find standard BLE list
- Here are 15 unique [**v4-UUIDs**](#15-unique-v4uuids) from the [Online UUID Generator](https://www.uuidgenerator.net/)
```
355d2b52-982c-4598-b9b4-c19156686e1a
    ...
```

- Helpful from Argenox:
  - [**Argenox website**](https://www.argenox.com/library/bluetooth-low-energy/ble-advertising-primer/) is a good place to READ about BLE
  - Here's the [Bluetooth Low Energy Library](https://www.argenox.com/library/bluetooth-low-energy/)
  - Here is a link for [BLE and batteries](https://www.argenox.com/library/bluetooth-low-energy/powering-ble-batt/)
- **Arduino** resources:
  - A complete [reference](https://www.arduino.cc/reference/en/)
  - And one specifically for [BLE](https://www.arduino.cc/reference/en/libraries/arduinoble/)
- Here's some GATT information [(_LINK_)](https://www.oreilly.com/library/view/getting-started-with/9781491900550/ch04.html) from O'Reilly (2014), and its repository, [(_here._)](https://github.com/microbuilder/IntroToBLE)
- Here's a 
helpful [**beginners tutorial**](https://devzone.nordicsemi.com/nordic/short-range-guides/b/bluetooth-low-energy/posts/ble-advertising-a-beginners-tutorial) from Nordic Semi. 
And another [**here.**](https://devzone.nordicsemi.com/nordic/short-range-guides/b/bluetooth-low-energy/posts/bluetooth-smart-and-the-nordics-softdevices-part-1)
- **nRF Connect:**
  - nRF Connect is good for testing and connecting. I don't know yet how it dovetails into specific app development, but using nRF Connect seems to be the right phone app to use for this.
  - In my case, I would set up my Arduino as the server, and the nRF Connect as the client. Because the server/sensor sends out information and the client receives it.
- Here's helpful [IMU and BLE](https://www.hackster.io/gov/imu-to-you-ae53e1) tutorial from hackster.io
- Wiki about [C data types](https://en.wikipedia.org/wiki/C_data_types#stdint.h)


#
#
#
#

# Try again:

- Go back to _golf-swing-acc_ and copy it as _new-test-imu-ble-combo_ (done)
  - **Create the sketch like adding pieces to a puzzle** (did this, done)

#### Examples:
BLE is basically done, so move important stuff to the [Reference](#reference) section.
- Go through all the _ArduinoBLE_ sketches **in the Examples folder in the IDE**
- Also use the [**Arduino guide for NANO33BLESense**](https://www.arduino.cc/en/Guide/NANO33BLESense) for reference
- Also go through the later lessons in _**EdX Deployment**_ class
- Here's a YouTube video ( [*Bluetooth BLE on ESP32 works! Tutorial for Arduino IDE*](https://youtu.be/osneajf7Xkg) ) that shows some detail about Server/Client and characteristics
  - and in which he mentions "BLE2902" but I can't find usage for it yet. But it showed up on nRF Connect "0x2902"

#### nRF Connect looks like this

(screenshot of my phone screen with device listed)
<p align="center"><img src="http://some_place.com/nrf-screenshot.png" /></p>

(video of device LED on and off when it tips on y-axis)
<p align="center"><img src="http://some_place.com/nrf-screenshot.png" /></p>


##### Next: [Modifying the file (first draft)](#modifying-the-file)

#
#
#
#
##### (good notes that should be incorporated into [Reference](#reference) section)
# New notes for modding the file:

Arduino's reference for BLE:
- **From https://www.arduino.cc/en/Reference/ArduinoBLE**

**Notify or Indicate.** Think of this as _Sender_ and _Reader_. ArduinoBLESense is the _sender_ and when a reading changes, the nRF Connect is going to be the _reader_ at the right moment. For my purposes, the _sender_ wants to let the _reader_ know that the state has changed from Ready to Resting, and vice versa. This reduces the BLE communication (which is the most energy-hungry part of this project) down to one single instance: _characteristic change_ (state change). The model BLE uses is known as a **publish-and-subscribe model.**

Sender/Arduino is _Peripheral/Server_, and Reader/nRF Connect is _Central/Client_

**Updating a characteristic.** When Y-axis, `y < -0.85`, changes from true to false or back, this is the moment to send BLE data, nothing else. Save on BLE energy. _Need to adopt energy-saving code later._

Interesting: There are two GATT units, 0x2743 and 0x2744, which are _angular velocity (radian per second)_ and _angular acceleration (radian per second squared)_, respectively. Don't know whether I'd be able to use this. It's related to centripetal force.

#
#
#
#

##### (See also: [New notes for modding the file](#new-notes-for-modding-the-file))

# Modifying the file:
## why is this here?
### code has been copied already, so just keep essential or unique stuff
Maybe this formatting is more readable?
##### most of this will be in the 'file structure' section (TRUE)
(do this entire process again, using different example BLE sketch) **(DONE!)**

( use this section to describe the _golf-sensors-acc_ ) (I think this section is unnecessary because it's going to end up in the "Structure of Arduino files" section)



Top of sketch. First, add the two libraries. (copy to Structure)
```
#include <ArduinoBLE.h>           // Bluetooth Library
#include <Arduino_LSM9DS1.h>      // IMU
```


Next, create the SERVICE name "180C". 
```
// BLE Service Name
BLEService customService("180C");
```

##### characteristic notes:
Next, add a specific CHARACTERISTIC. If it were a string, there would also be a number for its data length.
- "2A58" seems quite arbitrary and in other examples is actually the 128-bit UUID. Came from the example. _Each characteristic either DOES or DOES NOT need a unique UUID, so I'll have to **look this up** and why._ (I believe that a service has a unique UUID, and it's characteristics are also unique UUIDs.)
```
// BLE Characteristics
// Syntax: BLE<DATATYPE>Characteristic <NAME>(<UUID>, <PROPERTIES>, <DATA LENGTH>)
BLEFloatCharacteristic ble_magnetic("2A58", BLERead | BLENotify);
```


In `void setup()`, first check whether the services have started. (also [caveat](#caveat))
```
void setup() {
  Serial.begin(9600);
  while (!Serial);
  Serial.println("Started");
  
  // add BLE
  if (!BLE.begin()) {
    Serial.println("Failed to initialize BLE!");
    while (1);
  }
  
  // add IMU
  if (!IMU.begin()) {
    Serial.println("Failed to initialize IMU!");
    while (1);
  }
```


Create the name of the service to find in nRF Connect (done)
```
  // Setting BLE Name
  BLE.setLocalName("Arduino Environment Sensor");
```

##### char advert notes:
Tell the device to advertise the service (send info via BLE to the receiving end). 
Here, _"ble_magnetic"_ refers to the IMU readings, the accelerometer in our case. 
(_might change this from "magnetic" to "acc"_)
- This would be where more characteristics are added. 
Anything that's going to be sent to the smartphone via BLE would be added like this, under `customService.addCharacteristic(example_char)` and then accessed within later code and displayed using `example_char.writeValue()`. (true, good note)
```
  // Setting BLE Service Advertisment
  BLE.setAdvertisedService(customService);

  // Adding characteristics to BLE Service Advertisment
  customService.addCharacteristic(ble_magnetic);

  // Adding the service to the BLE stack
  BLE.addService(customService);
```


Invoke the BLE to advertise. And also go ahead and print that it's active. (done)
```
  // Start advertising
  BLE.advertise();
  Serial.println("Bluetooth device is now active, waiting for connections...");
```


That's the end of `void setup()`. I've left the original commands to print as-is from the original. (don't need)
```
  // IMU original stuff:
  Serial.print("Accelerometer sample rate = ");
  Serial.print(IMU.accelerationSampleRate());
  Serial.println(" Hz");
  Serial.println();
  Serial.println("Acceleration in G's");
  Serial.println("X\tY\tZ");
}
```

##### void loop notes:
- initialize variables in the void loop()

Now the `void loop()`. 
```
void loop() {
  float x, y, z; // IMU params
  // Variable to check if central device is connected
  BLEDevice central = BLE.central();

  if (central) {
    // Serial.print("Connected to central: ");
    // Serial.println(central.address());
```


Do these things _while_ BLE is connected. _(Read [the caveat](#caveat).)_ 
This `while` statement is why nothing shows up in Monitor until BLE connects the two devices. (_**not really**_) 
The `readValues()` is not used in this case, but in the _RoboCraze_ example, it combines readings and labels into a string
which can be read easily in nRF Connect with `writeValue(m)`. 
_( `readValues()` is a function; read [here](#structure-of-arduino-files) )_ `readValues()` is a subroutine to collect the x,y,z of the sensor, and combine it into a readable string.
**And the `readValues()` function executes from inside of the `while (central.connected())` loop.**

```
    while (central.connected()) {
      delay(200); // take this out if necessary

      // Read values from sensors
      //readValues();

      // Writing sensor values to the characteristic
      //ble_magnetic.writeValue(m);
      //delay(1000); // so we can read it
```


Next, using `readAcceleration()` and `writeValue()` sends information to the BLE App. (yes)

##### important: (good notes)
**I need to make this more readable.** I don't know why it writes as a HEX or ID. But the HEX changes as I move the device around, and slows to one second when in the _Resting_ state, meaning that it's properly functioning. **But the reading doesn't make sense.**
- It turns out that it's really easy to make Strings show up in the app. 
- I don't know whether I need any raw data for the current step.
  - In the code, yes/no on the threshold, triggers LED on/off, and "Ready"/"Resting" write to the nRF Connect app, as well as to the serial monitor.
  - So it might not need any raw data via BLE at all. All the action and calculations will be in the local code, nothing to do with BLE. Sending on/off signals for stuff is all it needs to do. If there's more of a purpose then I don't know what it is. We should keep this simple.
```
      // IMU checking on Y and printing all to Monitor
      if (IMU.accelerationAvailable()) {
        IMU.readAcceleration(x, y, z);

        ble_magnetic.writeValue(y);
        //delay(1000); // so we can read it
```


_This is where the y is read and Ready/Resting is established._
_These values for y need to get sent to BLE._ **(do they?)**
_So how are they converted/kept and sent?_ (really necessary?)

Then the same stuff from before. Including the one second pause that I mentioned.
```
        if ( y > -.85 ) {         // -1G is the threshold
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
    }
```


Next, this wraps up the `void loop()` and shows up in Monitor before the devices connect.
```
    Serial.print("Disconnected from central: ");
    Serial.println(central.address());

  }
```

_This is where x,y,z readings are combined and turned into text strings, rather than leaving it as RAW._

And down here is where the `readValues()` is. Used in the _RoboCraze_ example sketch.
```
//void readValues()
//{
//  m = y; // random try in case I understand this
  
    /*
    // Reading raw sensor values from three sensors
    float x, y, z;
    if (IMU.magneticFieldAvailable()) {
      IMU.readMagneticField(x, y, z);

    // Saving sensor values into a user presentable way with units
    m = "X:" + String(x) + ", Y:" + String(y);
    }
    */
//}
```

# end













#
#
#
#
#
#
#
# Finish connecting BLE 100% as planned before moving on to the SDK part
#
##### [Digging into app dev](#digging-into-app-dev)
(later move this section down here if appropriate)



#
##### Step Four:
## Enable Smartphone to BEEP

- Lookup: How to control Android with... (controller, another android, etc) and find some development apps?

#### Description
Proof of feasibility. Beep triggered by in/out of Ready state is not for final product, but good for this development, because there is other activity that will require prompting smartphone to act on something in some way.
- Requires App Development: Use [**nRF Connect SDK**](https://www.nordicsemi.com/Products/Development-software/nrf-connect-sdk)
- Enable smartphone functions via nRF (Requires SDK)
- What can be configured in my phone that receives commands from the nRF Connect application?
  - Can the phone app trigger **BEEP** or a vibration/buzz? (But should be just once, at change of its state)
  - Can the nRF App turn on/off the phone's **flashlight**? (_Good Idea!_)
- Make the smartphone beep in Ready state


#### Insert video
<p align="center"><img src="http://some_place.com/image.png" /></p>

##### Video of moving device back and forth, and hearing the beep sound from the phone


### Identifying a state change and taking action

What I want is a way for my Android to recognize a state change coming from the **arduino**. 
- When the state goes from 0 to 1, I want the phone's flashlight to turn on. When it goes from 1 to 0, should turn off.
- More directly, state change into and out of Ready/Resting states. If `y < -.85` then turn on the flashlight on my phone!
- There may be BLE-specific code that transmits _only_ when there's a state change, and could shorten this entirely

##### State change: (pseudo code)

```
resting = state("Resting");
ready = state("Ready");
now = update.state();         // returns "Ready" or "Resting"

if ( now !== earlier ) {      // if state has now changed
    if (now == resting) {     // and is now Resting
        beep(low);            // then beep low for new Resting state
        }
    else {
        beep(high);           // otherwise beep high for new Ready state
        }
    earlier=now;              // update earlier state with now state
    }
    pass;                     // now == earlier, so no state change
```


### Dog bark KWS example:
Sensor devices similar to the BLE Sense have been used to trigger audio to play from another device.
The dog barking example from YouTube (here) is one. 
So I need to use the matching code from that example and apply it for my purpose,
which is to get nRF Connect to trigger actions in my phone.
That microphone sensor created KWS model that triggered an app to play some prerecorded audio.
And I want my accelerometer to trigger my phone flashlight on/off, because it senses _Ready/Resting_ states.
- Found [**this**](https://youtu.be/v5hBjouFHQY) dog bark video about how the BLE Sense triggered other devices.
- Question is whether it's using BLE or some other connection. But it's a good example of KWS.
  - Wouldn't need BLE if listening device connects with wire to audio player!



#
#
#
#
##### (Step Four: Enable Smartphone to BEEP)
- When creating this sketch, we must create a new Development Sketch, "_dev-sdk-ble-pitches_".
- For the sketch here, since it's for immediate development, just create high and low pitches for the transitions into Ready and Resting states, respectively, and we'll save the code for future reference.
- Probably later create a third sketch that combines code from _golf-swing-acc_, the new inclusive sketch, and this _dev_ sketch.

## Reference Info:
- The all-inclusive Arduino file will be saved as _golf-sensors.ino_ when more sensors are involved.
  - Multiple "h" file can probably be included, to split off logically (see: [_magic wand_](#digging-deeper-into-the-magic-wand) example) from the _.ino_ file.
- For images, this is helpful: resizing and centering with `<p align="center"><img src="http://some_place.com/image.png" /></p>`
- Create an _interval_ for some sensor readings, using `millis()` not `delay()`. But `delay()` is good during Resting state, because all sensors are meant to be off.
- _**Magic Wand**_ [example](#digging-deeper-into-the-magic-wand)

#
#
### Digging deeper into the _magic wand_:
##### (more stuff)
- **LEARN** 
[from the course](https://learning.edx.org/course/course-v1:HarvardX+TinyML3+1T2021/block-v1:HarvardX+TinyML3+1T2021+type@sequential+block@e355a78c0dcd49b6acbeeaf8f7492859/block-v1:HarvardX+TinyML3+1T2021+type@vertical+block@6e2f8e18dd814e63ad68f60e380b6633)
about the _magic-wand_ sketch to see how the DATA is recorded there and what gets transmitted to the Serial Monitor, and then how that data displays on the Monitor from that data. What converts that data to the 'readable' visualization of the motion?
[**This** is the link to the course data collection browser app (use Chrome)](https://tinyml.seas.harvard.edu/magic_wand/).
- **TEST the motion of the gyro/acc.** Can this motion show up on the Plotter or Monitor? What does this motion look like for gyro/acc individually? Is it helpful to sample the data more slowly for better visualization?
- HOW does this data get recorded into a data point? We recorded a data set for the Exercise. So _how was that collected?_
- **_Can this activity happen LOCALLY?_** Because the exercise actually resided on _tinymlx.io_ or something. And that's where all the data got generated.
- **How much reliance upon external websites is necessary? Why not ALL local?**







#
#
#
# And then:
## AFTER enabling smartphone to beep when sensing Ready orientation
### QUESTION: What is the next physical step?
What is the Accelerometer doing at this point?
- When the Accelerometer is in the Ready state, another sensor (gyro, or maybe acc) identifies the Stillness state and prepares to record movement.
What are the specific physical instruments needed to determine whether the motion has stopped? 
- I could say, wait until all motion has stopped, but is there one in particular which 100% will say this? 
- It might just be the other 2 axes from the accelerometer. In this case, don't include the axis to which gravity is applied. Only use the other 2, and when they're below a threshold, they're still. (Having said that, I believe the gyro will be even more obvious)

#
##### Step Five:
## Get Gyro going

1. Figure out the Gyro data on Monitor.
1. Figure out how to collect gyro data.
1. Figure out how to **add** the KWS field ('yes'|null) to that data point.
1. Figure out how to combine data points into a **usable DATA SET** (with or without the KWS resolved)


#
##### Step Six:
## Collect gyro data

- Once in Ready state, figure out how to **enable the Gyro** to collect a sweep of data once motion begins.
- Watch Gyro data in Monitor. Collect X,Y,Z coordinates of Gyro, as well as TIME STAMPS (so, 4 dimensions)
  - As soon as Gyro reads that it's sitting still, that's when the collection can begin. 
- WHAT do Gyroscope readings represent? Are these what we want for our DATA COLLECTION?
  - Will fewer data points save memory? Is it necessary? (No, for now)
- Collect some data, and **then stop** when the Gyro is still again to SAVE THE DATA.


## More steps

- The gyro/acc record movement (_HOW MUCH MOVEMENT?_)
  - This data will require normalization, eliminating noise (LEARN)
  - Then a label must be applied to the recorded data points ('yes'|null)
- Then transmit the data points to smartphone
- Then enables the Accelerometer again, waiting to be in Ready state again



#
#

#### Repository Question
- Are libraries separate from this code? (probably yes) Libraries are listed within the code, so no need to describe more than _"verify you have all the libraries installed"_. 

#
#


### What are more parts to the project?

#### Figure out:
- how to determine amount of memory being used
- how to begin recording a golf swing
- how to finish recording a golf swing
- how to engage the KWS after the golf swing
- how to record the KWS label to the golf swing data (in data collection mode)
- how to build a dataset on my smartphone
- how to add the record to the dataset
- how to download a dataset of 100 records to PC
- how to develop and test a TinyML inference model from this dataset
- how to IMPLEMENT data inference to the device
- how to RECONFIGURE smartphone app from Ready state orientation to 'good swing' chirp
- finally, is it possible to simultaneously collect data and provide inference?

#### Next:
- how to gather components/parts list
- LEARN about power requirements
- LEARN about electronics configuration (physical)
- how to reduce form factor
- how to print schematic
- determine if need for 3D printing
- how to design PCB (KiCad)
- how to prototype (OSHPARK)
- testing prototype
- collect 10X more data


#
## More Learning:
- Repeat EdX course material, do all TinyML assignments again
  - Include Jupyter Notebooks, Python, C++ (the h files, c files and others)
  - **Fully utilize GitHub features and functionality**
- **TinyML Book** Create new repository "hello-world-arduino" for book exercise (sine function)


# [<-- back to Implementation](implementation.md)


## Also:
* Determine whether to keep the TEST site called _jdsgithubpages_ (probably not)
* Determine whether _ArduinoBLE-to-Android_ repository is necessary. (probably not) (DO NOT NEED _ArduinoBLE-to-Android_ so probably delete it.)
* Determine whether to add photo of Arduino project on Git Profile page as a GIF (probably not)
* Determine whether to use GitHub Desktop and/or BASH
* When needed, figure out how to incorporate JupyterLab Desktop
  * DOWNLOAD JupyterLab Desktop (look into this! or maybe just using browser tabs is okay?)
  * Determine whether some of this documentation would be better in the form of a _Jupyter notebook_
* Determine physical nature of MCU board form factor and its protection from damage
  * Prototyping with KiCad, OSHPARK, and more
  * Think about [**battery**](#battery-info)
* Determine whether my **GitHub Page** should be redirected to here from **tech**.jeffreysorgen.com 
  - Determine whether a **BLOG** is useful
  - Enable blogging somehow, with or without repositories, to publicly document actions taken (or just this page!)
* Not Needed: GIF of slideshow of certs, book, Arduino board, etc (not yet)
* Not Needed: **Wiki**
* "SWINGTASTIC"


#
### Jot down ideas for other projects here
- wind turbine ( Is the most popular product TinyML or IoT? )
- Continue recording _golf-swing-sensors_ progress in THIS repository
- **LED glasses** which display "HIGHLND" across them
  - **Acquire** one of the mounts/PCB boards, and maybe later design PCB myself. Requires Adafruit _feather_ board. I imagine I'd document a project just like this _golf-sensors_ one, step by step. Fun to imagine Patreon or YouTube connections also. I like the idea of offering this to fans and artists. Should be very fun to do, with profit potential.
  - **INCORPORATING _KWS_** for verbal commands! Like, "highlnd" would display "HIGHLND" across the frame, and could do "party", "love", "hearts" (heart emojis), and with special design specs I could do ANY voice commands. Of course limited to device capacity. Maybe not for the _feather_ but another that includes a microphone and capacity for TinyML.


#
# Thoughts and notes

## Fourier
- I would like to convert MOTION recorded here and stored in JSON into a _Fourier Series_ because it seems that that would be a good way to create 3D time-series illustrations
- ([3B1B Fourier Series video here](https://youtu.be/r6sGWTCMz2k?t=1226)) The video describes 2 dimensions, but what I'm interested in calculating is in 3 dimensions.
- This formula has hyperparameters which I believe can be learned by a Neural Net. A golf swing consists of a time series of 3D points (say, 100 3-dimensional points per second), and can entirely be drawn with a **Fourier Series!**
- Technically, although virtually impossible to visualize as a human being, this will work in _**n-dimensions!**_ 

### This means?
- that a _multi-dimensional_ line can be generated, against which comparisons can be made for a VERY QUICK INFERENCE!
- To illustrate, imagine a drone on a 3D path, and add 2 more dimensions (parameters) such as temperature and pressure. Now it's a 5-dimensional line against which a model can make a quick inference!
##
The above might describe some complete science fiction. I  may be thinking about this in the wrong way. After a very superficial review of Fourier Transform, I think that "3D" aspects are asking too much. I really don't think this is what I am looking for.

### Or it's possible that...
- all I really need is something that we learned about when we were analyzing spectrograms from AUDIO samples, and transforming them using **FFT (Fast Fourier Transform)** so maybe this can be applied to my problem, where there is a singular path (2D), but with one additional corresponding parameter.
- It's actually much like combining multiple sensors, isn't it?!! After all, an IMU with 9DOF actually has 9 readings, so of course being a mere human being, I couldn't see that we would just utilze what ML already knows how to do.

### So what am I looking for, anyway? (I'm exhausted.)
- I think all I'm actually looking for is a real-time 3d representation connecting all the data points of the golf swing. The ML can figure out all the other stuff on its own when I send it in the right direction.
- **is FFT useful here?** is this going to be like converting audio spectrograms?
#
##### 21oct22
If there were a way to CREATE a tiny microcontroller board which contains an IMU programmed the way I want to use it with the golf swing, then I can imagine that it might be useful in a wide variety of purposes.
These purposes would center around collecting data from the IMU as it moves very swiftly through the air, or even in a very small area.
I suppose it would be a bit like anomaly detection in the realm of machinery vibration.
But this is movement.
I'm trying to say that my knowledge from completing this golf club sensors project could extend into other areas.
I don't know... I'm just rambling. But I intended to record some thoughts here, about having a tiny MCU with BLE module and associated memory chips involved, and could broadcast its data (I'm trying to develop this now) to receiving points along the way. (I'm imagining a subway train for some reason, and dropping off data along its path, becoming refreshed so it can collect more data.)
This seems like just a data collection thing. It is. But I'd like to include ML inference AT THE SAME TIME. So, I would be able to collect information, apply a label to it based on its ML Categorization inference.

In the golf swing, data is collected one swing at a time. This is what I was trying to describe. (But it went to a subway train instead.) There must be good ideas about how this motion capture can be utilized in this way.

My project is designed to first collect data by labeling each swing myself, and expanding that dataset to having other people make the swing and label the swing good or not.
I am trying to simplify this process so much that it is possible for a high school golf coach to collect data.
And then I want to have that specific data incorporated into the available dataset, and then repeated, so that I can eventually collect the worlds first and largest dataset of good golf swings. 

**I know that's ambitious,** but it's a hard project and I want to get it done, for the experience.
#
Speaking about using ML to respond with an inference: I imagine a physically separate MCU which receives this data from MCU#1 as a data point. MCU1 generates the data, MCU2 makes an inference about it and sends that combination to MCU3 which collects that data. I am imagining MCU3 is the piece which stores the data, and then sends it via BLE to a receiving device which has more storage like IoT device, which can in turn send to the cloud.

Conceptually, this is still **the subway analogy.** But makes it easier to think about. Maybe this analogy is not a bad one while I create this. The golf swing is the subway route, club head is the subway car, device is attached to the surface of the subway car, and one swing is analogous to the route between stops with all its curves and speeds.

Using this analogy, it really seems like it's been done before: _self-driving cars,_ and that whole system. What are they using? Do they rely on GPS too much? Yes, in comparison, I think they do. But they do speed, distance, turns, bumps, motion, etc. And they collect data, and they make inferences along the way.

However, I am removing all the horsepower. I am greatly simplifying what's been done with giant compute power. If I can reduce this information into data points with a _shape_ under 10 (xyz axes, time stamp, plus other related params) then I'm going to be able to utilize MCU rather than the larger devices. And if I'm able to use this in the form of **Fourier Series** then it might be able to be **reduced further in the ML model.** Yes, I'm back to that again! I know there's complicated math. And anything beyond 2D is hard to describe. I don't yet understand the correlation with the FFT (fast fourier transform used for converting audio spectragrams) but I feel strongly that this is _important._
#
#
##### Immeditate needs:
I am hoping to capture the attention of someone. I don't know who or why or how, but I am trying to listen creatively where I can for opportunities.
