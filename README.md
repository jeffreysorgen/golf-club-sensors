# hello-world
Just another repository

Hello Git Community!
I am Jeff - new to Git, but __*not*__ new to tech and coding. I've used Python predominantly, but I'm beginning with TinyML and Arduino, so C++ is also going to be in the mix very soon.

*This hello-world repository was created to learn Git and store my progress and ideas.*

**Files:**
- My activity is documented in [activity.md](https://github.com/jeffreysorgen/hello-world/blob/7bd94fe77f16c863a019b92694e90602d6542bdf/activity.md) or [this](activity.md)
- My golf swing idea is documented in [golf-swing-idea.md](https://github.com/jeffreysorgen/hello-world/blob/7b07c321b45f928e86106f0fd99a9ded52c8264f/golf-swing-idea.md)

## Documenting my activity:

Completed:

1. **Git** Practicing with Repositories, Branches, Commits, Pull Requests, and Merges with [help from this Guide](https://guides.github.com/activities/hello-world/)
2. **Markdown** Learned to use [Markdown from this guide](https://guides.github.com/features/mastering-markdown/)
3. **TinyML** Took course for TinyML from EdX (complete)
4. This README describes my next steps and ideas as well as what's in this Completed list
5. This README defines the parts of my [golf swing idea](https://github.com/jeffreysorgen/hello-world/blob/main/README.md#golf-swing-idea) (ongoing)

Next steps:
* Create **GitHub Pages** with [**this** guide](https://guides.github.com/features/pages/)
  - LEARN about this (doing this now)
* Include in GitHubPages: Progress, Accomplishments, Highlights and Features
* Upon completion of EdX course, review the material and do all assignments again
  - Include Jupyter Notebooks, Python, C++ (the h files and others)
  - **Fully utilize GitHub features and functionality**
* Add Accomplishments to GitHub Pages
  * Took Class
  * Bought book
  * Document assignments or examples
  * Include photos of certs, book, Arduino board
    * make GIF of these
* Determine whether my **GitHub Page** should be redirected to here from **tech**.jeffreysorgen.com 
* Create new repository "hello-world-arduino" for book exercise (sine function)
* Jot down ideas for other projects here
  * wind turbine ( Is the most popular product TinyML or IoT? )
  * Continue to flesh out golf swing idea, _below_
* Prototyping with KiCad, OSHPARK, and more
* Determine physical nature of MCU board form factor and its protection from damage
* Determine whether learning SmartPhone App Development is necessary
* Not Needed: **Wiki**

## Golf Swing Idea

_This data flow will become clarified as I go through the EdX lessons a second time_

### Purpose:

There are two. First, data collection which is described here. Its architecture expects me to say 'yes' following a good swing. 

The other purpose is to use collected data for an ML model that beeps a signal when it identifies a good practice swing. **Unsure if it's possible to implement both purposes.**

### Physical Description:

Attach a small form factor MCU to the back of a golf club head securely to engage movement sensors.

### Technical Description:

This is a multi-tenancy model. The IMU and Microphone function in cascade fashion. The BLE is always enabled but only transmits during data collection. 
**The following describes a way to collect data for a golf swing.** 
It waits for the motion to stop, and then starts looking for the motion of a swing, and then I speak into the microphone 'yes' so that it gains a label, otherwise it is null. 
The null swings are much more frequent, because the system records everything that follows a pause of motion if the orientation of the device is at the starting position.
The BLE service provides a way to record data without restrictions. It's simply always on, without needing to go to a cloud service which is typically necessary for IoT devices, because the smartphone application collects the data. Only the smartphone needs to be near the device, within several feet, and the device does not need to connect to anything else. So this system is ideal for remote functionality, even without cellphone service, because a smartphone can connect via Bluetooth to the device without needing wifi or Cell service.

### Flow:

Uses BLE:
- Start by connecting the MCU to the smartphone (how is this done?)
- Begin IMU void loop

Uses IMU:
- If Event Data exists, send data via BLE and clear buffer
- IMU void loop waits for zero orientation on plane, 
  * then waits for motion to settle, 
  * then enables recording of swing motion (event loop) 
- record the motion event
  * store motion data
  * engage the KWS void loop 

Uses KWS:
- KWS void loop listens for 'yes'
- after delay, records null label then returns to IMU void loop
- after 'yes' Keyword, records Yes label then returns to IMU void loop



### Learning about this right now:
KiCad, OSHPARK, GitHub, probably GitBASH

#
*All I'm doing right now is working with the hello-world readme.md file to practice using Git. After building confidence just with the MD file, I'll start working with CODE*
