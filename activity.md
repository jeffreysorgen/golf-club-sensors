# Documenting my activity:
**Learning about this:**
- GitHub Desktop, KiCad, LTspice, OSHPARK, probably GitBASH
- Arduino Components and code requirements
## Completed:
1. **Git** Practicing with Repositories, Branches, Commits, Pull Requests, and Merges with [help from this Guide](https://guides.github.com/activities/hello-world/)
2. **Markdown** Learned to use [Markdown from this guide](https://guides.github.com/features/mastering-markdown/)
3. **TinyML** Took course for TinyML from EdX (complete)
4. [**activity.md**](activity.md) (This) Describes my next steps, ideas, and this Completed list
5. [**golf-swing-idea.md**](golf-swing-idea.md) Defines the parts of my golf swing idea (ongoing)
6. **GitHub Pages** Added using [this guide](https://guides.github.com/features/pages/) and is accessible [here](https://jeffreysorgen.github.io/hello-world/) (ongoing)
7. Changed hello-world (this) repository to **PRIVATE**, so it isn't publicly available now on GitHub Pages either
8. Installed **GitHub Desktop**, and added [_ArduinoBLE-to-Android_](https://github.com/jeffreysorgen/ArduinoBLE-to-Android) repository
9. Defined Golf Swing Flow, [_here_](#flow-for-golf-swing-sensors)



## Next steps:
### [Set up](#setting-up-and-testing-the-sensors) BLE and Magnetometer (and document)
**Learn how to edit Arduino sketches!** (Created _test_IMU_custom.ino_ sketch)
- __QUESTION__ What are the specific physical instruments needed to determine whether the motion has stopped. I could say, wait until all motion has stopped, but is there one in particular which 100% will say this? 
- **Slow down the sensor (do we need so many data points?)** In the SETUP loop, change the baud rate to lowest --> _Serial.begin(300);_ <-- from 9600
#### Need yes/no for orientation of device
Attach the device to a stick, oriented horizontally as shown. [image]

_! [Sm-attaching to a stick ] ( https: //github.com/jeffreysorgen/hello-world/blob/current-edits/images/Sm-attaching%20to%20a%20stick.png )_
#
![Sm-attaching to a stick](images/Sm-attaching%20to%20a%20stick.png){:height="50%" width="50%"}

# this.
#### Images: 1.Attach the device to a stick, 2.Start orientation, 3.Device orientation, 4.Rest orientation
1<img src="https://user-images.githubusercontent.com/1236972/135545687-3e1b9fda-1544-4802-93a2-572b97b9b99b.png" width="20%">
2<img src="https://user-images.githubusercontent.com/1236972/135545934-7cb4dd34-7c12-46b9-ae8f-fa2e61835812.png" width="20%">
3<img src="https://user-images.githubusercontent.com/1236972/135546061-106e68f4-bfba-4cd5-929e-49494486ad87.png" width="20%">
4<img src="https://user-images.githubusercontent.com/1236972/135545966-edb098fe-ab01-4e57-8c80-1988ad779186.png" width="20%">
- The graph between the start and rest orientation shows the difference in the readings of the Y axis when the device is attached _"the long way"_ where the red line is always greater than both or less than both other axes.

## .

[image] Start position, Y is less than both X and Z
[image] Readings of the magnetometer
[image] Rest position, where Y is greater than both X and Z

- use the Magnetometer readings to read its orientation
- The readings of the magnetometer, according to the setup in the images: if Y < X and Y < Z then orientation is in start position. So if y axis is less than the other two readings, then the device is in start position. The opposite is true. If Y is greater than the other 2 axes, then the device is idle and in rest, and is meant to WAIT for its orientation to return to the start position.
     



#
* Repeat EdX course material, do all TinyML assignments again
  - Include Jupyter Notebooks, Python, C++ (the h files, c files and others)
  - **Fully utilize GitHub features and functionality**






###
#### Future Steps:
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









### Setting up and testing the sensors:
Enable a sensor and TEST. Set up serial monitor displaying orientation, and signal "yes" that it senses that the orientation is at described 'zero' orientation. When it pulls away from that orientation, it continues to wait for it to get there again. And after 30 seconds, will stop looking. Pings the device every 5 to 10 seconds for that orientation, and remains idle until zero orientation is found.


#### To Do:
- Enable the magnetometer. Edit magnetometer sketch. Show Arduino is or is not oriented at 'zero'
  - Find magnetometer sketch
  - Upload sketch
  - Enable serial monitor
  - Watch orientation of magnetometer
  - Edit sketch to identify whether orientation is zero=yes or zero=no.
  - Upload sketch
  - Enable serial monitor and watch for "Yes"
  - Connect BLE to phone app
  - Watch phone app monitor for "Yes"
  - **Develop APP that can beep when it sees "Yes"**
    - [nRF Connect SDK](https://www.nordicsemi.com/Products/Development-software/nrf-connect-sdk)
###
- For BLE, watch [**this**](https://youtu.be/2q_tA8v5l1Y) video
- Implement BLE. (What code is used? Can it be included in this documentation?)
  - Move the computer to the table, then plug in the Arduino, find the correct ports
  - **LABEL** this device in the code. Upload the sketch.
  - Install the app in the smartphone, and connect to the BLE device
- connect to smartphone and read sensor within the phone app. Can this sensor trigger a Yes/No or On/Off? Can the phone app trigger a vibration or a beep? Is this where **App Development** comes in? Turn on/off the phone's flashlight?
- If possible, copy the code I use for this step into the repo, _ArduinoBLE-to-Android_
- **DOCUMENT** this activity and requirements. Are libraries separate from this code? (probably yes) Libraries are listed within the code, so no need to describe more than _"verify you have all the libraries installed"_. Decide whether to include these steps in a **BLOG**
- Determine whether to open new _golf-swing-steps.md_ to describe building process. This is out of the idea stage, and is now in the execution stage.

### Flow for [Golf Swing](golf-swing-idea.md) sensors:

- **BLE** is enabled when the device is powered, waits for pairing forever.
- Pairs with phone app.
- Clear device buffer and **begin loop**
- Enable mag/acc/gyro
- Mag/acc/gyro wait for stillness (no movement)
- Mag then identifies direction of gravity=zero.
- Acc/gyro records movement. 
- **Collection mode:** After movement, enables microphone.
  - **Microphone** listens for 3 seconds, and records Null, unless it hears "yes" (**This is KWS**)
  - Send data points to phone app with label. (_Is collection and inference possible together?_)
- _**Inference mode:** Checks data points with **MODEL**_
  - _Beep/buzz if good swing_
  - _Silent (or low tone) if no match_

