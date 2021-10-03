[<-- back to Implementation](implementation.md)

[<-- back to README.md](new-readme.md)
# To Do:

## Edit SKETCH and upload: to display "Ready" and/or "Resting" in the Serial Monitor
- LEARN how to use boolean logic within the sketch
  - This lesson will transfer to other processes
  - Set up thresholds?
  - Simple as Y>X or Y>Z ?
- Print "Ready" for when the device senses its Start orientation
- Print "Resting" for when the device senses its Rest orientation

## LEARN how to enable both BLE and Magnetometer
- **Enable BOTH Magnetometer and BLE into new custom sketch**
- **Test** Magnetometer, that it works as before

## Enable smartphone nRF Connect App via BLE
1. Download to Android (done)
2. **LABEL** this device in the code, upload the SKETCH
4. Pair the device
5. **WATCH** what the Serial Monitor displays ("Ready" to "Rest" and back again)

## Enable smartphone functions via nRF (Requires SDK)
### Make the smartphone beep in Ready state
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
- Created _**new-readme.md**_ that is __EXACTLY__ what I want for new repository, _golf-swing-sensors_ (ongoing)
  - __RENAME__ this _hello_world_ repository to _golf-swing-sensors
  - Then backup this (old) _readme_ and rename _new-readme.md_ to become the new **readme.md** for this repository

## Also:
* DOWNLOAD JupyterLab Desktop (look into this!)
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
### I would like to convert MOTION recorded here and stored in JSON into a Fourier Series because it seems that would be a good way to create 3D time-series illustrations

#
#
#
#
