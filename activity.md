[_[ <-- back to README ]_](README.md)
[_[ <-- back to Implementation ]_](implementation.md)

[_[ jump to Battery Info --> ]_](#battery-info)

## To Do:
- Save a copy of the example sketch as _ble-sense_
- Test it
- Copy the code into _golf-swing-acc_

[*[ Part Two: Enable BLE ]*](#part-two-enabling-ble)
[*[ Enable BLE and Accelerometer together ]*](#enable-accelerometer-together)
[*[ Part Three: Enable Smartphone to BEEP ]*](#enable-smartphone-to-beep)


## Part Two: Enabling BLE

#### Description

Before we can get it to chirp in response to a good or bad swing, the smartphone needs to pair up with the Arduino BLE Sense. We should be able to see on my Android whatever information we've already sent to the serial monitor. To do this, Nordic has an app that I downloaded from Google Play called _nRF Connect_. I'm going to go through the process of getting that started right here.

### Try the BLE example sketch

I found -[_**this video**_](https://youtu.be/2q_tA8v5l1Y)- from _Robocraze_ to be helpful, 
and copied the `.ino` code from its [_accompanying GitHub repository_](https://github.com/Robocraze/Nano-33-BLE-Examples/blob/43fbe5b3155493d3056e85d7402c54e05c84f133/environment_sensor_ble/environment_sensor_ble.ino).
Then I right-clicked on _Raw_, and saved the file, 
and then created a folder with the same name to put it into, because that's what Arduino requires.

I'm now going to upload the sketch to the device...
**And it worked exactly as it does in that video.**

#### To do this:

1. Download _nRF Connect_ from **Google Play** (also available for iOS)
2. Find the `environment_sensor_ble.ino` file from the _Robocraze_ repository and copy it locally.
3. Upload the _environment_sensor_ble_ sketch to the device
4. Open the serial monitor, watch for "Disconnected from central..."
5. (App) Open the phone app, _nRF Connect_, and enable Bluetooth
6. (App) Scan for and connect to the device (its name was declared in the sketch)
7. (App) Touch _UnknownService, UUID: 0x180C_
8. Watch Monitor again for services to pop up
9. (App) Touch the "triple down arrow" <img src="images/3downarrows.png" width="20em" /> for each of the three services for this example
10. (App) OBSERVE the temperature gradually reach ambient room temperature or hold in hand for it to rise

### Combining BLE and IMU commands in the IDE
- Copy _golf-swing-acc_ as _test-imu-sketch_ (done)
- Communicate with smartphone by adding BLE functionality, line by line (done)
  - Repeatedly upload sketch to device looking for errors and functionality
  - **RESULT!** But data is not in the form of a float, _but a hex_
    - _So how do I convert this?_
- To do: Go through all the sketches **in the Examples folder in the IDE**
  - Also use [**the arduino guide for NANO33BLESense**](https://www.arduino.cc/en/Guide/NANO33BLESense) for reference

#
##### Next, 
- **figure out how to deliver readable data to phone screen** using arduino examples to figure it out
- go through the process of importing the settings for BLE to the "acc" sketch
- go through the process of setting up the battery
#
1. Find the sketch in the Examples
2. Edit the sketch, include the label, _"ble-sense"_
3. Upload to the Arduino
4. (If the code is simple enough, then just incorporate it into _golf-swing-acc_)
5. THEN: Download the app from Google Play (done)
6. Enable Bluetooth on my phone
7. Scan for devices in nRF Connect

### nRF Connect looks like this
(screenshot of my phone screen with device listed)
<p align="center"><img src="http://some_place.com/nrf-screenshot.png" /></p>

### Battery
Now that we can see that it's been paired, we can talk about our [battery solution](#battery-info). Originally I had to stay tethered to my computer. But now I have a lightweight phone recharger with the same USB-C connection.
<p align="center"><img src="http://some_place.com/battery-connection.png" /></p>




#
## Enable Accelerometer together
- Enable BOTH Accelerometer and BLE into _new_ custom sketch
  - _This could have been done in the prior step!_
- TEST the Accelerometer, that it works as before
  - WATCH that the Android displays "Ready" to "Resting" and back again
  - Get screenshot and post here.
- Utilize BATTERY SOLUTION described [*here*](#battery-info) or see [the setup *here*](#battery)

# Finish connecting BLE 100% as planned before moving on to the SDK part


## Enable Smartphone to BEEP
- When creating this sketch, we must create a new Development Sketch, "_dev-sdk-ble-BLE-pitches_".
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


## Battery Info:
### Prototyping solution
- NEED connect to USBmicro female from the Arduino to a power source.
- PURCHASED a lightweight **PHONE CHARGER** to serve this purpose during development. 
  - So now I can connect with BLE and be _physically detached_ from my computer!
  - Would just need a strap or pocket or something for it. 
- Later can build an obviously better solution.  
- When 100% finished prototyping with my Arduino Nano 33 BLE Sense, I will be looking into using a different board, and a battery solution will definitely be a part of the research.
  - I am looking for those **2-prong** "magnetic" battery chargers, what kind of battery is in that fit-watch, and where to get that rechargable battery. 
  - **Qi coil** is a wireless charging device.


## Reference Info:
- The all-inclusive Arduino file will be saved as _golf-sensors.ino_ when more sensors are involved.
- For images, this is helpful: resizing and centering with `<p align="center"><img src="http://some_place.com/image.png" /></p>`
- Create an _interval_ for some sensor readings, using `millis()` not `delay()`. But `delay()` is good during Resting state, because all sensors are meant to be off.


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


# more stuff

- **LEARN** 
[from the course](https://learning.edx.org/course/course-v1:HarvardX+TinyML3+1T2021/block-v1:HarvardX+TinyML3+1T2021+type@sequential+block@e355a78c0dcd49b6acbeeaf8f7492859/block-v1:HarvardX+TinyML3+1T2021+type@vertical+block@6e2f8e18dd814e63ad68f60e380b6633)
about the _magic-wand_ sketch to see how the DATA is recorded there and what gets transmitted to the Serial Monitor, and then how that data displays on the Monitor from that data. What converts that data to the 'readable' visualization of the motion?
[**This** is the link to the course data collection browser app (use Chrome)](https://tinyml.seas.harvard.edu/magic_wand/).
- **TEST the motion of the gyro/acc.** Can this motion show up on the Plotter or Monitor? What does this motion look like for gyro/acc individually? Is it helpful to sample the data more slowly for better visualization?
- HOW does this data get recorded into a data point? We recorded a data set for the Exercise. So _how was that collected?_
- **_Can this activity happen LOCALLY?_** Because the exercise actually resided on _tinymlx.io_ or something. And that's where all the data got generated.
- **How much reliance upon external websites is necessary? Why not ALL local?**


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
* Jot down ideas for other projects here
  * wind turbine ( Is the most popular product TinyML or IoT? )
  * Continue recording _golf-swing-sensors_ progress in THIS repository
* Not Needed: GIF of slideshow of certs, book, Arduino board, etc (not yet)
* Not Needed: **Wiki**
* "SWINGTASTIC"
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
