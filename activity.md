- add photo of battery/old phone arrangement
- try a new example for BLE!

[*[ <-back to Overview ]*](README.md)
[*[ (previous) Step One: The Accelerometer ]*](implementation.md/#step-one-the-accelerometer)
**[[ Step Two: Enable BLE ]](#step-two-enabling-ble)**
[*[ Battery solution ]*](#power-solutions)
[*[ Step Three: BLE+IMU together ]*](#enable-accelerometer-together)
[*[ Step Four: Enable Smartphone to BEEP ]*](#enable-smartphone-to-beep)
[_[ jump to new project ideas-> ]_](#jot-down-ideas-for-other-projects-here)

## Step Two: Enabling BLE

#### Description
Before we can get it to chirp in response to a good or bad swing, the smartphone needs to pair up with the Arduino BLE Sense. We should be able to see on my Android whatever information we've already sent to the serial monitor. To do this, Nordic has an app that I downloaded from Google Play called _nRF Connect_. I'm going to go through the process of getting that started right here.

### Try the BLE example sketch
###### [_(TL;DR)_](#tldr-1)
I found -[_**this video**_](https://youtu.be/2q_tA8v5l1Y)- from _Robocraze_ to be helpful, 
and copied the code from its [**accompanying GitHub repository**](https://github.com/Robocraze/Nano-33-BLE-Examples/blob/43fbe5b3155493d3056e85d7402c54e05c84f133/environment_sensor_ble/environment_sensor_ble.ino).
This example reads information from the sensors and then simply displays it in the phone app. Upload the sketch to the device... _**And it works exactly as it does in that video.**_ But there is a [*caveat*](#caveat) I discovered for this example.

#### To do this:

1. Download _nRF Connect_ from **Google Play** (also available for iOS)
2. Find the `environment_sensor_ble.ino` file from the _Robocraze_ repository and copy it locally. 
_(Right-click on _Raw_, save the file, and drop into same-name folder, as required by Arduino.)_
3. Upload the _environment_sensor_ble_ sketch to the device
4. Open the serial monitor, watch for "Disconnected from central..."
5. (App) Open the phone app, _nRF Connect_, and enable Bluetooth
6. (App) Scan for and connect to the device (its name was declared in the sketch)
7. (App) Touch _UnknownService, UUID: 0x180C_
8. Watch Monitor again for services to pop up
9. (App) Touch the "triple down arrow" <img src="images/3downarrows.png" width="20em" /> for each of the three services for this example
10. (App) OBSERVE the temperature gradually reach ambient room temperature or hold in hand for it to rise
##### Caveat:
- If USB cable remains plugged into the computer I **can** discover _"Arduino Environment Sensor"_ in nRF Connect
- Using a **battery-only** solution, was **not** able to discover _"Arduino Environment Sensor"_ in nRF Connect 
- For the **[_magic-wand_](#digging-deeper-into-the-magic-wand)** example, using battery-only solution, the computer **can** discover the BLE service as expected
- I could troubleshoot the _RoboCraze_ code, but I am going to find a different example instead
  - Evidently, the _RoboCraze_ solution kept the cable plugged into the computer, and seems to be reliant on that particular configuration (edit this)
  - nRF Connect on Phone --> Arduino which is still powered by computer USB cable (edit this)
  - I don't know whether there was truly a disconnection there? (edit this)
- _nRF Connect_ isn't the only phone app that might suit my purpose, but it _**is** Nordic Semi_
##### *(TL;DR)*
*We should try out other BLE examples, and find a BLE sketch that works better than [the one](#try-the-ble-example-sketch) from _RoboCraze_. This example doesn't work battery-only, and importing line-by-line from that example into my IMU sketch produces the same problem. While it would be good to learn how to transform from hex value into a readable one, I'm inclined to do that later, if it's still a problem, after I've explored some other examples.*


##### (Battery Info)
## Power Solutions

### Future prototyping solution
- Later can build an obviously better solution.  
- When 100% finished developing with my Arduino Nano 33 BLE Sense, I will be looking into using a different board for prototyping, and a battery solution will definitely be a part of the research.
  - **The board needs to include (1) an IMU, (2) a microphone, and (3) a solvable battery option**
- Battery options:
  - I am looking for those **2-prong** "magnetic" battery chargers, what kind of battery is in that fit-watch, and where to get that rechargable battery. 
  - **Qi coil** is a wireless charging device.
  - **CR1220** is a small, common coin-type battery
  - **LIR2032H** is a common 3.7 rechargable, but 20mm, so like a nickel size.

### Current development solution
- **Connect with only BLE and be _physically detached_ from the computer.**
- Attach the Arduino Sense (USBmicro port) to a power source.
- Use a lightweight **phone recharger** (with 2 USB out) to serve this purpose during development.  
- Rechargers will shut off after a short time with just a low power drain, so this won't work by itself.
  - **Charging an _old phone_ at the same time will prevent this auto-shutoff** 

##### Charging up a dead old phone prevents auto-shutoff
<p align="center"><img src="http://some_place.com/pic-of-battery-solution.png" /></p>


### Alternative development solution
- This is not practical for _golf-club-sensors_ project but is helpful information nonetheless.
- There's a power solution in the TinyML Course, attaching a 9V battery to the **Learning Kit Shield**. 
  - This [**Appendix**](https://github.com/tinyMLx/appendix/blob/main/PoweringArduino.md#battery) is a good place to read about it.
  - It's certainly not designed for swinging around, but it is proof that there's a pinout solution.
  - It's a good example for a stationary device.

#






#
#
## Finding a simple BLE solution
### We now have a [development](#current-development-solution) power solution to untether ourselves from our computer.
### nRF Connect
- nRF Connect is good for testing and connecting. I don't know yet how it dovetails into specific app development, but using nRF Connect seems to be the right phone app to use for this.
- In my case, I would set up my Arduino as the server, and the nRF Connect as the client. Because the server sends out information and the client receives it.
### Try again:
- Go back to _golf-swing-acc_ and copy it as _new-test-imu-ble-combo_
- **Go through example sketches to find simplest one to integrate BLE and my IMU code so far**
- Communicate with smartphone by adding BLE functionality, line by line
- Repeatedly upload sketch to device looking for errors and functionality (DO THIS with a different example)
#### Examples:
- Go through all the _ArduinoBLE_ sketches **in the Examples folder in the IDE** 
- Also use the [**Arduino guide for NANO33BLESense**](https://www.arduino.cc/en/Guide/NANO33BLESense) for reference
- Also go through the later lessons in _**EdX Deployment**_ class
- Here's a YouTube video: [Bluetooth BLE on ESP32 works! Tutorial for Arduino IDE](https://youtu.be/osneajf7Xkg)
  - Shows some detail about Server/Client and characteristics


#
### Combining BLE and IMU commands in the IDE
(old section)
- Copy _golf-swing-acc_ as _test-imu-sketch_ (done)
- Communicate with smartphone by adding BLE functionality, line by line (done)
  - (I used the _RoboCraze_ example so this still won't work battery-only)
  - Repeatedly upload sketch to device looking for errors and functionality (DO THIS with a different example)
  - **_RESULT!_** But data is not in the form of a float, _but a hex_ (n/a for now)
    - _So how do I convert this?_ (n/a for now)
    - Check to see if this works with the battery solution. (doesn't)
- **Make this look better** (n/a for now)
  - **figure out** how to deliver readable data to phone screen using arduino examples to figure it out (n/a for now)
  - Go through the process of importing the settings for BLE to the "acc" sketch (n/a for now)
  - Go through all the _ArduinoBLE_ sketches **in the Examples folder in the IDE** (yes!) 
  - Also use the [**Arduino guide for NANO33BLESense**](https://www.arduino.cc/en/Guide/NANO33BLESense) for reference (yes!)
  - Also go through the later lessons in _**EdX Deployment**_ class (yes!)
  - Find out whether this needs to become **_peripheral_ rather than _central_** (skip)
  - More below, in the ["Next"](#next) section (skip/edit)



#
#### See: [Modifying the file](#modifying-the-file)





### BLE+IMU notes
I am trying to figure out which BLE settings in the IDE to use so that I can make the nRF Connect readings display "Ready" / "Resting". 
Once that's done, get my phone to turn its flashlight on/off as a result. 
What we want to do for this project is to read information from the sensor and then get the phone app to act upon the capabilities of the phone, such as turning on a flashight or beeping. 
While the flashlight functionality won't be used in the end, that solution is crucial for when we're trying to get the phone to chirp good/bad golf swings. 


#
#

## Enable Accelerometer together
- Enable BOTH Accelerometer and BLE into _new_ custom sketch
  - _This could have been done in the prior step!_
- TEST the Accelerometer, that it works as before
  - WATCH that the Android displays "Ready" to "Resting" and back again
  - Get screenshot and post here.

#### nRF Connect looks like this
(screenshot of my phone screen with device listed)
<p align="center"><img src="http://some_place.com/nrf-screenshot.png" /></p>


#
#
#

## Modifying the file:
(do this entire process again, using different example BLE sketch)

#

Top of sketch. First, add the two libraries.
```
#include <ArduinoBLE.h>           // Bluetooth Library
#include <Arduino_LSM9DS1.h>      // IMU
```
Next, create the SERVICE name "180C".
- _Don't know why it's "180C" just that it came from the example and shows up in nRF Connect. Possibly default ID for BLE Service? I don't know._
```
// BLE Service Name
BLEService customService("180C");
```
Next, add a specific CHARACTERISTIC. If it were a string, there would also be a number for its data length.
- "2A58" seems quite arbitrary and in other examples is actually the 128-bit UUID. Came from the example. _Each characteristic either DOES or DOES NOT need a unique UUID, so I'll have to **look this up** and why._
```
// BLE Characteristics
// Syntax: BLE<DATATYPE>Characteristic <NAME>(<UUID>, <PROPERTIES>, <DATA LENGTH>)
BLEFloatCharacteristic ble_magnetic("2A58", BLERead | BLENotify);
```
In `void setup()`, first check whether the services have started.
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
Create the name of the service to find in nRF Connect
```
  // Setting BLE Name
  BLE.setLocalName("Arduino Environment Sensor");
```
Tell the device to advertise the service (send info via BLE to the receiving end). Here, _"ble_magnetic"_ refers to the IMU readings, the accelerometer in our case. (_might change this from "magnetic" to "acc"_)
- This would be where more characteristics are added. Anything that's going to be sent to the smartphone via BLE would be added like this, under `customService.addCharacteristic(example_char)` and then accessed within later code and displayed using `example_char.writeValue()`.
```
  // Setting BLE Service Advertisment
  BLE.setAdvertisedService(customService);

  // Adding characteristics to BLE Service Advertisment
  customService.addCharacteristic(ble_magnetic);

  // Adding the service to the BLE stack
  BLE.addService(customService);
```
Invoke the BLE to advertise. And also go ahead and print that it's active. It's printed once at the top of Monitor, but scrolls quickly up. (edit this)
```
  // Start advertising
  BLE.advertise();
  Serial.println("Bluetooth device is now active, waiting for connections...");
```
That's the end of `void setup()`. I've left the original commands to print as-is from the original. 
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
Do these things _while_ BLE is connected. This `while` statement is why nothing shows up in Monitor until BLE connects the two devices. The `readValues()` is not used in this case, but in the _RoboCraze_ example, it combines readings and labels into a string which can be read easily in nRF Connect with `writeValue(m)`.
```
    while (central.connected()) {
      delay(200); // take this out if necessary

      // Read values from sensors
      //readValues();

      // Writing sensor values to the characteristic
      //ble_magnetic.writeValue(m);
      //delay(1000); // so we can read it
```
Next, using `readAcceleration()` and `writeValue()` sends information to the BLE App. **I need to make this more readable.** I don't know why it writes as a HEX or ID. But the HEX changes as I move the device around, and slows to one second when in the _Resting_ state, meaning that it's properly functioning. **But the reading doesn't make sense.**
```
      // IMU checking on Y and printing all to Monitor
      if (IMU.accelerationAvailable()) {
        IMU.readAcceleration(x, y, z);

        ble_magnetic.writeValue(y);
        //delay(1000); // so we can read it
```
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

#
#

#
# Finish connecting BLE 100% as planned before moving on to the SDK part
#

#
## Enable Smartphone to BEEP

**So for Part Four:**
- Lookup: How to control Android with... (controller, another android, etc) and find some development apps?

### Dog bark KWS example:
Sensor devices similar to the BLE Sense have been used to trigger audio to play from another device.
The dog barking example from YouTube (here) is one. 
So I need to use the matching code from that example and apply it for my purpose,
which is to get nRF Connect to trigger actions in my phone.
That microphone sensor created KWS model that triggered an app to play some prerecorded audio.
And I want my accelerometer to trigger my phone flashlight on/off, because it senses _Ready/Resting_ states.
- Found [**this**](https://youtu.be/v5hBjouFHQY) video about how the BLE Sense triggered other devices.


### Identifying a state change and taking action
What I want is a way for my Android to recognize a state change coming from the **arduino**. 
- When the state goes from 0 to 1, I want the phone's flashlight to turn on. When it goes from 1 to 0, should turn off.
- More directly, state change into and out of Ready/Resting states. If `y < -.85` then turn on the flashlight on my phone!

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

#
### Step Four: Enable Smartphone to BEEP
- When creating this sketch, we must create a new Development Sketch, "_dev-sdk-ble-pitches_".
- For the sketch here, since it's for immediate development, just create high and low pitches for the transitions into Ready and Resting states, respectively, and we'll save the code for future reference.
- Probably later create a third sketch that combines code from _golf-swing-acc_, the new inclusive sketch, and this _dev_ sketch.

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

## Reference Info:
- The all-inclusive Arduino file will be saved as _golf-sensors.ino_ when more sensors are involved.
- For images, this is helpful: resizing and centering with `<p align="center"><img src="http://some_place.com/image.png" /></p>`
- Create an _interval_ for some sensor readings, using `millis()` not `delay()`. But `delay()` is good during Resting state, because all sensors are meant to be off.
- Found [**this**](https://youtu.be/v5hBjouFHQY) video about how the BLE Sense triggered other devices. (dog bark example)
- _**Magic Wand**_ [example](#digging-deeper-into-the-magic-wand)


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
# And then:
## AFTER enabling smartphone to beep when sensing Ready orientation
### QUESTION: What is the next physical step?
What is the Accelerometer doing at this point?
- When the Accelerometer is in the Ready state, another sensor (gyro, or maybe acc) identifies the Stillness state and prepares to record movement.
What are the specific physical instruments needed to determine whether the motion has stopped? 
- I could say, wait until all motion has stopped, but is there one in particular which 100% will say this? 
- It might just be the other 2 axes from the accelerometer. In this case, don't include the axis to which gravity is applied. Only use the other 2, and when they're below a threshold, they're still. (Having said that, I believe the gyro will be even more obvious)


#
# more todo:
1. Get Gyro going.
1. Figure out the Gyro data on Monitor.
1. Figure out how to collect gyro data.
1. Figure out how to **add** the KWS field ('yes'|null) to that data point.
1. Figure out how to combine data points into a **usable DATA SET** (with or without the KWS resolved)


## Collect gyro data
- Once in Ready state, figure out how to **enable the Gyro** to collect a sweep of data once motion begins.
- Watch Gyro data in Monitor. Collect X,Y,Z coordinates of Gyro, as well as TIME STAMPS (so, 4 dimensions)
  - As soon as Gyro reads that it's sitting still, that's when the collection can begin. 
- WHAT do Gyroscope readings represent? Are these what we want for our DATA COLLECTION?
  - Will fewer data points save memory? Is it necessary? (No, for now)
- Collect some data, and **then stop** when the Gyro is still again to SAVE THE DATA.



## Then:

- The gyro/acc record movement (_HOW MUCH MOVEMENT?_)
  - This data will require normalization, eliminating noise (LEARN)
  - Then a label must be applied to the recorded data points ('yes'|null)
- Then transmit the data points to smartphone
- Then enables the Accelerometer again, waiting to be in Ready state again

#### Repository Question
- Are libraries separate from this code? (probably yes) Libraries are listed within the code, so no need to describe more than _"verify you have all the libraries installed"_. 


#
## More steps
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
- all I really need is something that we learned about when we were analyzing spectrograms from AUDIO samples, and transforming them using FFT (Fast Fourier Transform) so maybe this can be applied to my problem, where there is a singular path (2D), but with one additional corresponding parameter.
- It's actually much like combining multiple sensors, isn't it?!! After all, an IMU with 9DOF actually has 9 readings, so of course being a mere human being, I couldn't see that we would just utilze what ML already knows how to do.

### So what am I looking for, anyway? (I'm exhausted.)
- I think all I'm actually looking for is a real-time 3d representation connecting all the data points of the golf swing. The ML can figure out all the other stuff on its own when I send it in the right direction.
- **is FFT useful here?** is this going to be like converting audio spectrograms?
#
