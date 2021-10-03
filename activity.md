[<-- back to Implementation](implementation.md)

[<-- back to README.md](new-readme.md)
# To Do:


## Edit SKETCH and upload: to display "Ready" and/or "Resting" in the Serial Monitor
- Use boolean logic within the sketch _(How? Is this needed?)_
- "Ready" for when the device senses its Start orientation
- "Resting" for when the device senses its Rest orientation

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

#### Repository Question
- Are libraries separate from this code? (probably yes) Libraries are listed within the code, so no need to describe more than _"verify you have all the libraries installed"_. 



#
## More Learning:
- Repeat EdX course material, do all TinyML assignments again
  - Include Jupyter Notebooks, Python, C++ (the h files, c files and others)
  - **Fully utilize GitHub features and functionality**
- TinyML Book
  - Create new repository "hello-world-arduino" for book exercise (sine function)
- What are GISTS ?
- Determine whether learning SmartPhone App Development is necessary (might be!)

## Next Steps
#### (done) 
- Created _**new-readme.md**_ that is __EXACTLY__ what I want for new repository, _golf-swing-sensors_ (ongoing)
  - __RENAME__ this _hello_world_ repository to _golf-swing-sensors
  - Then backup this (old) _readme_ and rename _new-readme.md_ to become the new **readme.md** for this repository

## Also:
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
#
#
#
#
