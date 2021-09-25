# hello-world
Just another repository

Hello Git Community!

I am Jeff, new here. But __*not*__ new to tech and coding. Python predominantly, and beginning with TinyML and Arduino so far, so cpp is also going to be in the mix.

## Documenting my activity:

My process:

1. **Git** Practicing with Repositories, Branches, Commits, Pull Requests, and Merges with [help from this Guide](https://guides.github.com/activities/hello-world/)
2. **Markdown** Learned to use [Markdown from this guide](https://guides.github.com/features/mastering-markdown/)
3. **TinyML** Took course for TinyML from EdX (complete)
1. Define the parts of [golf swing idea (ongoing)](https://github.com/jeffreysorgen/hello-world/blob/main/README.md#golf-swing-idea)

Next steps:
* Create **Git Page** with [**this** guide](https://guides.github.com/features/pages/) to show Accomplishments
* Upon completion of EdX course, review the material and do all assignments again
* Add Accomplishments to **Page**
  * Took Class
  * Bought book
  * Document assignments or examples
  * Include photos of certs, book, Arduino board
    * make GIF of these
* Determine whether **Page** should be redirected here from **tech**.jeffreysorgen.com 
* OPTIONAL: Create **Wiki** showing Accomplishments
* Create new repository "hello-world-arduino"
* Jot down ideas for projects here
  * wind turbine
    * is the popular product TinyML or IoT ?
  * Continue to flesh out golf swing idea, _below_

### Golf Swing Idea

_This data flow will become clarified as I go through the EdX lessons a second time_

This is a multi-tenancy model. Except for the BLE, the IMU and Microphone function in cascade fashion. 
The following describes a way of collecting data for a golf swing. 
It waits for the motion to stop, and then starts looking for the motion of a swing, and then I speak into the microphone 'yes' so that it gains a label, otherwise it is null. 
The null swings are much more frequent, because the system records everything that follows a pause of motion if the orientation of the device is at the starting position.
The BLE service provides a way to record data without restrictions. It's simply always on, without needing to go to a cloud service which is typically necessary for IoT devices, because the smartphone application collects the data. Only the smartphone needs to be near the device, within several feet, and the device does not need to connect to anything else. So this system is ideal for remote functionality, even without cellphone service, because a smartphone can connect via Bluetooth to the device without needing wifi or Cell service.

Uses IMU:
* IMU void loop waits until motion halts, orientation on plane, sets up to record swing motion (event loop)
* MCU records motion event
  - stores motion data
  - engages KWS loop 

Uses KWS:
- KWS void loop listens for 'yes'
- null records label then returns to IMU void loop
- 'yes' records label then returns to IMU void loop

Uses BLE:
- data fills available memory with specific number of motion events
- device connects with BLE and prompts signal from smartphone
- waits to connect
- drops data off
- clears data out
- returns to IMU void loop

### Learning right now:
KiCad, OSHPARK, GitHub

#
*All I'm doing right now is working with the hello-world readme.md file to practice using Git. After building confidence just with the MD file, I'll start working with CODE*
