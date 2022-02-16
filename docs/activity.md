[*[ Overview ]*](README.md/#golf-swing-sensors)
[*[ 1 The Accelerometer ]*](implementation.md/#the-accelerometer)
[*[ 2 Solve for Power ]*](implementation.md/#solve-for-power)
**[*[ 3 Enable BLE ]*](EnablingBLE.md#step-three)**
[*[ Step Four: Enable Smartphone Response ]*](EnablingBLE.md/#step-four)
[*[ Step 4.5: Enable KWS ]*](KWS.md/#key-word-spotting)
[*[ Enabling the Gyro ]*](#steps-five-and-six)
[*[ Recording the Data ]*](#steps-five-and-six)
[_[ jump to new project ideas-> ]_](thoughtsandnotes.md/#other-projects)

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

- Steps
  - _**First, get serial monitor going and get it to print out data points**_
  - It can print out data points / plot, but can it collect them and print out after movement stops?
  - It's possible to send "every sesson" of movement via BLE, if I can figure out how to RECORD the bytes, which are the x,y,z,t coordinates
    - And once it has been send via BLE to a computer, can be compiled into a data set






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

# So...
### Now that we have Resting and Ready states:
If in Resting State, loop continues with two second delay between checks
If in Ready State, more things happen.
In Ready State, the club head is either moving or it is not.
- Check if club head is still

#### Change Serial Monitor to show data
Is the x,y,z showing "zero"? No. It is showing GPS-type information. It's showing **orientation.**
Use the Gyro to show that it's sitting still.
Use the Gyro to show that it's moving.
While Device is in Ready State, determine if the Gyro is sitting still.
Within the code loop which is in Ready State, use the Gyro data to determine whether the Device is moving.










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
