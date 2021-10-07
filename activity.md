[<-- back to Implementation](implementation.md)

[<-- back to README.md](README.md)
# To Do:
## CREATE NEW Arduino SKETCH:
### REPEAT testing the IMU (SimpleAccelerometer)
- Save as _golf-sensors.ino_ 
- Do standard test. Watch the plot.
- Determine whether to delete X and Z in the sketch, because only Y is needed
- Set up if statements { if Y < -.8 ...etc } then print as "Resting"
- Also, then don't keep printing, until not Resting state
- In the sketch, create a delay of 10ms prior to the reading! This would sample 100 per second.



## Edit SKETCH and upload: to display "Ready" and/or "Resting" in the Serial Monitor (in progress)
- LEARN how to use boolean logic within the sketch. _This lesson will transfer to other processes._
  - Set up thresholds?
  - Simple as Y>X or Y>Z ?
- Print "Ready" for when the device senses its Start orientation
- Print "Resting" for when the device senses its Rest orientation


# Then:
## LEARN how to enable both BLE and Magnetometer
- **Enable BOTH Magnetometer and BLE into _new_ custom sketch**
- **Test** Magnetometer, that it works as before



## Enable smartphone nRF Connect App via BLE
1. Download to Android (done)
2. **LABEL** this device in the code, upload the SKETCH
4. Pair the device
5. **WATCH** what the Serial Monitor displays ("Ready" to "Rest" and back again)

## Enable smartphone functions via nRF (Requires SDK)
### Make the smartphone beep in Ready state
Proof of feasibility. Not used in final product. But other activity will require prompting a smartphone to act on something in some way.
#### Research:
- For BLE, watch [**this**](https://youtu.be/2q_tA8v5l1Y) video
- App Development
  - Use [nRF Connect SDK](https://www.nordicsemi.com/Products/Development-software/nrf-connect-sdk)
- What can be configured in my phone that receives commands from the nRF Connect application?
- **TEST:** 
  - Can the phone app trigger **BEEP** or a vibration/buzz? 
  - Can the nRF App turn on/off the phone's flashlight? (_Good Idea!_)

## AFTER enabling smartphone to beep when sensing Ready orientation
#### QUESTION: What is the next physical step?
- What are the specific physical instruments needed to determine whether the motion has stopped? 
  - I could say, wait until all motion has stopped, but is there one in particular which 100% will say this? 
- What is the Magnetometer doing at this point?
  - Nothing is needed at this point, but should not mistake readings to be _Rest orientation_
  - When the Magnetometer is in the Ready state, it turns off (?) and then turns on the gyro/acc

##
- **LEARN** 
[from the course](https://learning.edx.org/course/course-v1:HarvardX+TinyML3+1T2021/block-v1:HarvardX+TinyML3+1T2021+type@sequential+block@e355a78c0dcd49b6acbeeaf8f7492859/block-v1:HarvardX+TinyML3+1T2021+type@vertical+block@6e2f8e18dd814e63ad68f60e380b6633)
about the _magic-wand_ sketch to see how the DATA is recorded there and what gets transmitted to the Serial Monitor, and then how that data displays on the Monitor from that data. What converts that data to the 'readable' visualization of the motion?
[**This** is the link to the course data collection browser app (use Chrome)](https://tinyml.seas.harvard.edu/magic_wand/).
  - **TEST the motion of the gyro/acc.** Can this motion show up on the Plotter or Monitor? What does this motion look like for gyro/acc individually? Is it helpful to sample the data more slowly for better visualization?
  - HOW does this data get recorded into a data point? We recorded a data set for the Exercise. So _how was that collected?_
  - **_Can this activity happen LOCALLY?_** Because the exercise actually resided on _tinymlx.io_ or something. And that's where all the data got generated.
    - **How much reliance upon external websites is necessary? Why not ALL local?**
#### Then:
- The gyro/acc record movement (_HOW MUCH MOVEMENT?_)
  - This data will require normalization, eliminating noise (LEARN)
  - Then a label must be applied to the recorded data points ('yes'|null)
- Then transmit the data points to smartphone
- Then enables the Magnetometer again, waiting to be in Ready state again

#### Repository Question
- Are libraries separate from this code? (probably yes) Libraries are listed within the code, so no need to describe more than _"verify you have all the libraries installed"_. 

## More steps
### What are more parts to the project?
#### Figure out:
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
- FIGURE OUT: don't mess up with GITHUB!
- how to gather components/parts list
- LEARN about power requirements
- DESIGN battery solution
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
- What are GISTS ?
- Determine whether learning SmartPhone App Development is necessary (might be!)

## Next Steps
#### (done) 
- Renamed repository

[<-- back to Implementation](implementation.md)
## Also:
* Determine whether some of this documentation would be better in the form of a _Jupyter notebook_
* Delete golf-swing-idea.md because I've incorporated it already
* Delete README-old.md because I don't need it
* DOWNLOAD JupyterLab Desktop (look into this! or maybe just using browser tabs is okay?)
* Determine whether to use GitHub Desktop and/or BASH
* Continue recording progress in THIS page
* Jot down ideas for other projects here
  * wind turbine ( Is the most popular product TinyML or IoT? )
  * Continue to flesh out golf swing idea (golf-swing-idea.md)
* Prototyping with KiCad, OSHPARK, and more
* Determine whether _ArduinoBLE-to-Android_ repository is necessary. (probably not)
  - DO NOT NEED _ArduinoBLE-to-Android_ so probably delete it.
* Determine whether to keep the TEST site called _jdsgithubpages_ (probably not)
  - But decide whether repo _golf-swing-sensors_ needs to convert to public
* Determine whether my **GitHub Page** should be redirected to here from **tech**.jeffreysorgen.com 
  - Enable blogging somehow, with or without repositories, to publicly document actions taken (or just this page!)
* Determine physical nature of MCU board form factor and its protection from damage
* Determine whether a **BLOG** is useful
* Not Needed: GIF of photos of certs, book, Arduino board, etc (not yet)
* Not Needed: **Wiki**
* "SWINGTASTIC"
#
# 
### I would like to convert MOTION recorded here and stored in JSON into a _Fourier Series_ because it seems that that would be a good way to create 3D time-series illustrations
([3B1B Fourier Series video here](https://youtu.be/r6sGWTCMz2k?t=1226)) The video describes 2 dimensions, but what I'm interested in calculating is in 3 dimensions.

This formula has hyperparameters which I believe can be learned by a Neural Net. A golf swing consists of a time series of 3D points (say, 100 3-dimensional points per second), and can entirely be drawn with a **Fourier Series!**

Technically, although virtually impossible to visualize as a human being, this will work in _**n-dimensions!**_ 

### This means that a _multi-dimensional_ line can be generated, against which comparisons can be made for a VERY QUICK INFERENCE!
To illustrate, imagine a drone on a 3D path, and add 2 more dimensions (parameters) such as temperature and pressure. Now it's a 5-dimensional line against which a model can make a quick inference!
#
The above might describe some complete science fiction. I  may be thinking about this in the wrong way. After a very superficial review of Fourier Transform, I think that "3D" aspects are asking too much. I really don't think this is what I am looking for.

It's possible that all I really need is something that we learned about when we were analyzing spectrograms from AUDIO samples, and transforming them using FFT (Fast Fourier Transform) so maybe this can be applied to my problem, where there is a singular path (2D), but with one additional corresponding parameter.

**It's actually much like combining multiple sensors, isn't it?!! After all, an IMU with 9DOF actually has 9 readings, so of course being a mere human being, I couldn't see that we would just utilze what ML already knows how to do.**

So what am I looking for, anyway? (I'm exhausted.)

I think all I'm actually looking for is a real-time 3d representation connecting all the data points of the golf swing. The ML can figure out all the other stuff on its own when I send it in the right direction.

#
#
#
