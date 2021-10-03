[<-- back to Implementation](implementation.md)

[<-- back to README.md](new-readme.md)
# To Do:



### Make the Serial Monitor show the word "Yes" or "Ready" when the device senses its Start orientation, and the word "Resting" when it senses its Rest orientation
- Enable this activity within the Arduino IDE
- Use boolean logic within the sketch _(how?)_
#### Then make the smartphone beep when Ready
- _HOW?_
- Set up BLE and connect to phone
- Open monitor in phone, and watch magnetometer state go from Ready to Resting, and back
  - _What can be configured in my phone that receives commands from the nCF Connect application?_
- **Develop APP that can beep when it sees "Yes"**
  -  [nRF Connect SDK](https://www.nordicsemi.com/Products/Development-software/nrf-connect-sdk)






#
#

### Set up BLE and IMU (and document)
- __QUESTION__ (to ask after working out how to make the phone beep when sensing Ready orientation)
  - What are the specific physical instruments needed to determine whether the motion has stopped? I could say, wait until all motion has stopped, but is there one in particular which 100% will say this? 
### Setting up and testing the sensors:
Enable a sensor and TEST. (no kidding)
Set up serial monitor displaying orientation, and signal "yes" that it senses that the orientation is at described 'zero' orientation. (this is the logic that I'm going to be setting up next)
When it pulls away from that orientation, it continues to wait for it to get there again. (this is true, and I think it's been stated above)
And after 30 seconds, will stop looking.  (Really? or is it always-on mode?)
Pings the device every 5 to 10 seconds for that orientation, and remains idle until zero orientation is found. (Maybe doesn't need to ping anything, as long as it's 'on' when in Ready mode and 'off' when in Resting mode.)
#

#### To Do:
- Enable the magnetometer. Edit magnetometer sketch. Show Arduino is or is not oriented at 'zero' (x=got this info)
  - Find magnetometer sketch x
  - Upload sketch x
  - Enable serial monitor x
  - Watch orientation of magnetometer x
  - Edit sketch to identify whether orientation is zero=yes or zero=no. x-working on it
  - Upload sketch
  - Enable serial monitor and watch for "Yes" x
  - Connect BLE to phone app x=said it
  - Watch phone app monitor for "Yes" x
  - **Develop APP that can beep when it sees "Yes"**
    - [nRF Connect SDK](https://www.nordicsemi.com/Products/Development-software/nrf-connect-sdk) x
###
- For BLE, watch [**this**](https://youtu.be/2q_tA8v5l1Y) video
- Implement BLE. (What code is used? Can it be included in this documentation?)
  - Move the computer to the table, then plug in the Arduino, find the correct ports x
  - **LABEL** this device in the code. Upload the sketch. (label for the BLE connection)
  - Install the app in the smartphone, and connect to the BLE device
- connect to smartphone and read sensor within the phone app. Can this sensor trigger a Yes/No or On/Off? Can the phone app trigger a vibration or a beep? Is this where **App Development** comes in? x 
- TEST: Turn on/off the phone's flashlight? (Good Idea!)
- If possible, copy the code I use for this step into the repo, _ArduinoBLE-to-Android_ (or wherever, ongoing)
- **DOCUMENT** this activity and requirements. Are libraries separate from this code? (probably yes) Libraries are listed within the code, so no need to describe more than _"verify you have all the libraries installed"_. Decide whether to include these steps in a **BLOG**
- Determine whether to open new _golf-swing-steps.md_ to describe building process. This is out of the idea stage, and is now in the execution stage. (documentation elsewhere depends upon how complex this doc is, and I don't think it's complex enough)



#
###
#### Future Steps:
* Repeat EdX course material, do all TinyML assignments again
  - Include Jupyter Notebooks, Python, C++ (the h files, c files and others)
  - **Fully utilize GitHub features and functionality**

###

* Create _**new-readme.md**_ that is __EXACTLY__ what I want for new repository, _golf-swing-helper_
  - by combining the current .md files with the local _golf swing TinyML.txt_ 

###

* __RENAME__ this _hello_world_ repository to _golf-swing-helper_
  - Then backup this (old) _readme_ and rename _new-readme.md_ to become the new **readme.md** for this repository
* Create new repository "hello-world-arduino" for book exercise (sine function)
* Determine whether to use GitHub Desktop and/or BASH
* Continue recording progress in THIS page
  - Accomplishments or Highlights likely to be ANOTHER page
  - What are GISTS ?
* Jot down ideas for other projects here
  * wind turbine ( Is the most popular product TinyML or IoT? )
  * Continue to flesh out golf swing idea, [_here_](golf-swing-idea.md)
* Prototyping with KiCad, OSHPARK, and more
* Determine whether to keep the TEST site called _jdsgithubpages_
* Determine whether my **GitHub Page** should be redirected to here from **tech**.jeffreysorgen.com 
  - Enable blogging somehow, with or without repositories, to publicly document actions taken
* Determine physical nature of MCU board form factor and its protection from damage
* Determine whether learning SmartPhone App Development is necessary
* Not Needed: GIF of photos of certs, book, Arduino board, etc (not yet)
* Not Needed: **Wiki**




