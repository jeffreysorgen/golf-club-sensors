[*[ Overview ]*](README.md/#golf-swing-sensors)
[*[ 1 The Accelerometer ]*](implementation.md/#the-accelerometer)
[*[ 2 Solve for Power ]*](implementation.md/#solve-for-power)
**[*[ 3 Enable BLE ]*](EnablingBLE.md#step-three)**
[*[ Step Four: Enable Smartphone Response ]*](EnablingBLE.md/#step-four)
[*[ Step 4.5: Enable KWS ]*](KWS.md/#key-word-spotting)
[*[ Enabling the Gyro ]*](#steps-five-and-six)
[*[ Recording the Data ]*](#steps-five-and-six)
[_[ jump to new project ideas-> ]_](thoughtsandnotes.md/#other-projects)

**This page is now "Reference, etc."**
- [Reference](#reference)
- [UUID Info](#uuid-info)
- [Magic Wand Info](#magic-wand)
- [( <-- Enable Smartphone Response )](EnablingBLE.md/#step-four)
- [Enabling the gyro](#steps-five-and-six)
- [Recording the data](#steps-five-and-six) 

##
# Reference:

- Here's the okdo.com example, including [_BLE Hello World_](#the-ble-hello-world-sketch): [**getting started** from *okdo.com*](https://www.okdo.com/getting-started/get-started-with-arduino-nano-33-ble/#h-1-configure-ide-toc)
- **Hackster** tutorial: Here's a helpful [IMU and BLE](https://www.hackster.io/gov/imu-to-you-ae53e1) tutorial from hackster.io
- Wiki about [C data types](https://en.wikipedia.org/wiki/C_data_types#stdint.h)
- Helpful from Argenox:
  - [**Argenox website**](https://www.argenox.com/library/bluetooth-low-energy/ble-advertising-primer/) is a good place to READ about BLE
  - Here's the [Bluetooth Low Energy Library](https://www.argenox.com/library/bluetooth-low-energy/)
  - Here is a link for [BLE and batteries](https://www.argenox.com/library/bluetooth-low-energy/powering-ble-batt/)
- **Arduino** resources:
  - Arduino's reference for BLE](#arduinos-reference-for-ble) _(double check this link)_
  - Go through all the _ArduinoBLE_ sketches **in the Examples folder in the IDE**
  - Also use the [**Arduino guide for NANO33BLESense**](https://www.arduino.cc/en/Guide/NANO33BLESense) for reference
  - A complete [reference](https://www.arduino.cc/reference/en/)
  - And one specifically for [BLE](https://www.arduino.cc/reference/en/libraries/arduinoble/)
- **Nordic Semi** resources:
  - Helpful [**beginners tutorial**](https://devzone.nordicsemi.com/nordic/short-range-guides/b/bluetooth-low-energy/posts/ble-advertising-a-beginners-tutorial) from Nordic Semi. 
  - And another [**here.**](https://devzone.nordicsemi.com/nordic/short-range-guides/b/bluetooth-low-energy/posts/bluetooth-smart-and-the-nordics-softdevices-part-1)
#### _Reader and Sender:_ 
- Definition from [ArduinoBLE Reference](https://www.arduino.cc/en/Reference/ArduinoBLE)
- Think of this as _Sender_ and _Reader_. 
ArduinoBLESense device is the _sender_ (Peripheral). 
When a reading changes, the nRF Connect app is going to be the _reader_ (Client).
The model BLE uses is known as a "publish-and-subscribe" model.
#### _Coding Tips:_
- The all-inclusive Arduino file will be saved as _golf-sensors.ino_ when more sensors are involved.
  - Multiple "h" file can probably be included, to split off logically (see: [_magic wand_](#magic-wand) example) from the _.ino_ file.
- For images, this is helpful: resizing and centering with `<p align="center"><img src="http://some_place.com/image.png" /></p>`
- Change LED from one state to the other: `digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));`

##
## UUID Info:

- [Online UUID Generator](https://www.uuidgenerator.net/) created the [unique UUIDs](#15-unique-v4uuids)
- Need to have specific UUIDs for each IMU param
- Can't find a specific standard UUID for x,y,z on the Accelerometer
  - check _GATT Angulars_ note, [**here**](#gatt-angulars) 
  - what does the **raw** IMU data look like?
- Here's some GATT information [(_LINK_)](https://www.oreilly.com/library/view/getting-started-with/9781491900550/ch04.html) from O'Reilly (2014), and its repository, [(_here._)](https://github.com/microbuilder/IntroToBLE)
- Check this hackster.io post, [**here**](https://www.hackster.io/gov/imu-to-you-ae53e1), again
- Changed to 16-bit in the code, "FFE0", etc.

##### 15 unique v4UUIDs:
```
355d2b52-982c-4598-b9b4-c19156686e1a    // for imuUUID (ffe0)
9e5982a7-9ef0-48e0-a167-8112ada5f184    // for accUUID (ffe1)
9dc52af2-d585-4fb7-93a7-922b463239fe    // for stateUUID (ffe2)
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

**More UUID-Specific info:**

##### GATT angulars:
**Interesting:** There are two GATT units, 0x2743 and 0x2744, which are _angular velocity (radian per second)_ and _angular acceleration (radian per second squared)_, respectively. Don't know whether I'd be able to use this. It's related to centripetal force. _But if I use this measure, it will appear in the MCU code and not transmitted via BLE._

##### UUID BLE specification:
_I read that this was important for BLE:_ UUID for BLE: _"XXXXXXXX-0000-1000-8000-00805F9B34FB"_ _(Look this up to find standard BLE list)_

##
## magic wand
- **LEARN** 
[from the course](https://learning.edx.org/course/course-v1:HarvardX+TinyML3+1T2021/block-v1:HarvardX+TinyML3+1T2021+type@sequential+block@e355a78c0dcd49b6acbeeaf8f7492859/block-v1:HarvardX+TinyML3+1T2021+type@vertical+block@6e2f8e18dd814e63ad68f60e380b6633)
about the _magic-wand_ sketch to see how the DATA is recorded there and what gets transmitted to the Serial Monitor, and then how that data displays on the Monitor from that data. What converts that data to the 'readable' visualization of the motion?
[**This** is the link to the course data collection browser app (use Chrome)](https://tinyml.seas.harvard.edu/magic_wand/).
- **TEST the motion of the gyro/acc.** Can this motion show up on the Plotter or Monitor? What does this motion look like for gyro/acc individually? Is it helpful to sample the data more slowly for better visualization?
- HOW does this data get recorded into a data point? We recorded a data set for the Exercise. So _how was that collected?_
- **_Can this activity happen LOCALLY?_** Because the exercise actually resided on _tinymlx.io_ or something. And that's where all the data got generated.
- **How much reliance upon external websites is necessary? Why not ALL local?**

##

#

#

##
**Link to: [*[ Step Four: Enable Smartphone Response ]*](EnablingBLE.md/#step-four)**

# And...
**_Now that we've created an Android app:_**




##
# steps five and six
## Where we left off with the Accelerometer:

**What is the Accelerometer doing at this point?**
- When the Accelerometer is in the Ready state, another sensor (gyro, or maybe acc) identifies the Stillness state and prepares to record movement.

##### Record data
We need to have a starting point for when to record data. Then we need to record 100 or 500 points of data, using physical space orientation or movement. So what are the xyz coordinates when the accelerometer printed out information? Record this data every 20ms to start with.

**Where does this data go? Can it be stored within the Arduino Nano 33 BLE Sense?** And then how to access it? What data can we collect? Once we collect data, can we spit it out to the Serial Monitor? Can we collect multiple instances of the swing?

- How to collect a series of data points
  - Find moment in accelerometer where, even though it's in Ready State, it is "still" enough to register to begin recording data
  - Once "still", start recording data points, every 100ms, for 4 seconds. (This is 10 points of data per second, 4 seconds, which is 40 data points, and each data point is \[x,y,z,t], where t is the time stamp in ms from beginning of measurement.)
  - Still State is priority over recording. If during recording the readings are again at Still, then reset and wait for movement again.
  - From Still State goes backswing direction, followed by x,y,z,t as compared to its previous "location"
  - Data point 0 is t=0ms. Data point 1 is t=100ms and has moved X distance in this direction from point 0. So, x1 - x0 = x for this data point.
  - So one data point (p1) gets stored, and the next data point (p2) is in relation to prior data point. Data point p2 contains x,y,z, and a unique t compared to the other data points. One data record contains 160 bytes (_is this right?_) of data, which is x,y,z,t times 4 seconds, times 10 data points per second.
  - Swing is measured in 5000ms, or 5 seconds. Once it ends, wait for Still State.

- How to identify Still State
  - Device is already in Ready State
  - Device is moving around measurably - as if waving in the air
  - Device stops moving (within threshold of being still) - as if stationary upon a surface.
  - Next, Device waits to discover movement and the first data point is recorded at x0,y0,z0,t00ms
  - Next, second data point is recorded at x1,y1,z1,t100ms
  - This continues for five seconds.

- Trim Data
  - Every swing begins with Still State followed by a backswing, swing, and then in 5 seconds stops recording.
  - If Device detects motion becoming much slower on average, trim data here. (How to calc average? Distance between two data points is shorter than prior pair of data points on average.)  

- Trimmed Data has a 3D shape and speed
  - Every swing can be layered upon another in different colors for example, and will create "normal" swing
  - Outliers will be those which have almost no acceleration, the distance between data points is very small. (Nobody waves a club around as much as a practice swing or a real swing.)
  - Difference between a Practice Swing and a Real Swing is the hitting of a ball, which will spike the accelerometer at around the fastest point of the swing.

- How to record sample data
  - On a table, Device is stationary
  - Wave the device back and forth and then stop
  - 40 data points should print out (per second), and no other information printed (Serial Monitor)
  - Print swing to Serial Monitor, don't print non-swing
  - For this example, when Device moves, print the information.
  - But if it stops prior to 4 seconds, then it's Still State again, so don't print, will have fewer data points.
  - Serial Monitor should print nothing except for a completed record of 4 seconds of movement.
  - Print occurs when Still State is reached
  - Motion, then Still State, then print data points of that Motion

##### Question is: Is this the right sensor? Or is it a gyro?
Can find this out by recording sample data

- What happens to data while power is on? Is data persistant so I can plug Device into my computer to find it? Probably retrieve a dump of C code?
- Once "buffer" (EEPROM?) is full, records over old data as FIFO
- **How to retrieve the data as a dataset???**
- First, DOES the code record retrievable data?
- If yes, then HOW MUCH data gets recorded?

**Notes:**

What are the specific physical instruments needed to determine whether the motion has stopped? 
- I could say, wait until all motion has stopped, but is there one in particular which 100% will say this? 
- It might just be the other 2 axes from the accelerometer. In this case, don't include the axis to which gravity is applied. Only use the other 2, and when they're below a threshold, they're still. (Having said that, I believe the gyro will be even more obvious)

##### _DO THIS_
- First, find a way to stop and start the recording of data.
- Once recording starts, record 100 3D data points and corresponding time stamp.
- Once 100 data points are recorded, **SAVE THE DATA**




##
##### Step Five:
## Get Gyro going

1. Figure out the Gyro data on Monitor.
2. Figure out how to collect gyro data.
3. Figure out how to **add** the KWS field ('yes'|null) to that data point.
4. Figure out how to combine data points into a **usable DATA SET** (with or without the KWS resolved)

##
##### Step Six:
## Collect gyro data

- Once in Ready state, figure out how to **enable the Gyro** to collect a sweep of data once motion begins.
- Watch Gyro data in Monitor. Collect X,Y,Z coordinates of Gyro, as well as TIME STAMPS (so, 4 dimensions)
  - As soon as Gyro reads that it's sitting still, that's when the collection can begin. 
- WHAT do Gyroscope readings represent? Are these what we want for our DATA COLLECTION?
  - Will fewer data points save memory? Is it necessary? (No, for now)
- Collect some data, and **then stop** when the Gyro is still again to SAVE THE DATA.
  - Like when speed slows from fastest by 90 percent.
## More steps
- The gyro/acc record movement (_HOW MUCH MOVEMENT?_)
  - This data will require normalization, eliminating noise (LEARN)
  - Then a label must be applied to the recorded data points ('yes'|null)
- Then transmit the data points to smartphone
- Then enables the Accelerometer again, waiting to be in Ready state again
- _Smartphone to transmit to universal dataset in the cloud_ ([final step](#final-step))

##
## final step

**After collecting gyro/KWS data:**

**_Accumulate all the data._**
The final step of the project is figuring out how to send the collected data to a pool where we can use it to generate a universal dataset for **machine learning** so that we can improve our model. 
There is no user-specific information in the data being collected, so the sky is the limit.
Collecting all the data. This step will be after data collection is sorted out, and PCB prototyping is being considered.

And [here's a recent article](https://www.hackster.io/monica/analyze-accelerometer-data-on-maaxboard-with-edge-impulse-b7f28c) from hackster.io that may or may not be useful.

**But first, do steps [five](#step-five) and [six](#step-six)**

##

#

#

##
## What are more parts to the project?

**Figure out:**

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

**Next:**

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

##

#

#

##
## More Learning:
- Repeat EdX course material, do all TinyML assignments again
  - Include Jupyter Notebooks, Python, C++ (the h files, c files and others)
  - **Fully utilize GitHub features and functionality**
- **TinyML Book** Create new repository "hello-world-arduino" for book exercise (sine function)

##

[**_[ <-- back to Implementation ]_**](implementation.md)

##
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


##
## [**_[ Thoughts and Notes --> ]_**](thoughtsandnotes.md)
##

#

#

#

##
## Stuff I'm no longer considering but might be useful:

- There may be BLE-specific code that transmits _only_ when there's a state change, and could shorten this entirely, but for now I built it into this code. 
- Is there a way for the client to ask the peripheral whether the state has changed? Maybe. But how frequently and how much power consumption. Of course, the peripheral could ignore requests for update as well. Unless there's another way to think about this, I don't think this matters much. No savings of effort or energy.
- Another way to look at this is by doing a check on **whether the states match** on the peripheral and client, and if it doesn't, to update the client, although it might require more communication between devices.

##

**For nRF Connect Development:**

_We've connected the device to the nRF Connect App, and now it's time to figure out how to get a response from it._
- nRF Connect App Development: 
Use [**nRF Connect SDK**](https://www.nordicsemi.com/Products/Development-software/nrf-connect-sdk)
- I need nRF Connect for Desktop: 
[link](https://www.nordicsemi.com/Products/Development-tools/nRF-Connect-for-desktop/Download?lang=en#infotabs)
- There is a nRF Connect for VS Code, downloadable from the Toolchain Manager in nRF Connect for Desktop: 
[link](https://www.nordicsemi.com/Products/Development-tools/nRF-Connect-for-VS-Code/Download#infotabs)
- It looks complicated, but the videos were pretty clear (and recently made). The **videos** for installation are here on [_YouTube._](https://youtu.be/2cv_jjqk5hg) Might be more from a recent webinar November 3.

##

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

##
