# Documenting my activity:
- [Learning about this](#learning-about-this)
- [Completed](#completed)
- [Next Steps](#next-steps)
  - [flow for golf swing sensors](#flow-for-golf-swing-sensors)
  - [setting up and testing the sensors](#setting-up-and-testing-the-sensors)
## Learning about this:
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
8. Installed **GitHub Desktop**, added [_ArduinoBLE-to-Android_](https://github.com/jeffreysorgen/ArduinoBLE-to-Android) repository via Desktop



## Next steps:

* Develop Flow for Golf Swing sensors - [flow for golf swing sensors](#flow-for-golf-swing-sensors)
* Develop Testing for Golf Swing sensors
###
* Repeat EdX course material, do all TinyML assignments again
  - Include Jupyter Notebooks, Python, C++ (the h files, c files and others)
  - **Fully utilize GitHub features and functionality**




###
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




#### Flow for [Golf Swing](golf-swing-idea.md) sensors:
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

#### Setting up and testing the sensors:
Enable a sensor and TEST. Set up serial monitor displaying orientation, and signal "yes" that it senses that the orientation is at described 'zero' orientation. When it pulls away from that orientation, it continues to wait for it to get there again. And after 30 seconds, will stop looking. Pings the device every 5 to 10 seconds for that orientation, and remains idle until zero orientation is found.
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


