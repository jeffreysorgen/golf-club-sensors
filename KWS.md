[activity.md](activity.md)
[EnablingBLE.md](EnablingBLE.md)

#### Key Word Spotting
- Need to bump up the application for key word spotting. (break this section out independently)
- Enable the microphone on the Arduino to do this.
- Start using **TensorFlow Micro**.
- Which step is this going to be? Before enabling gyro? Collect gyro data first?
- Can this be applied after app development stage? After I get the phone to beep when the sensor reaches threshold, can it listen for key word and act on the sensor output to make the smartphone turn on the flashlight? Listen for "on"/"off" or "yes"/"no" and then turn the LED on or off?
- When in Ready state, turn on Gyro, finish Gyro motion, _then listen for key word_.
  - So do gyro step first. But before data collection?
  - Where does KWS fit in the flow?

Steps:
- App Development: make phone beep for Ready/Resting state
- Enable gyro when in Ready state, and start recording information when gyro readings "SETTLE"
- Study how gyro works. What do the readings look like? When I wave around the device, are the readings moving around? Do the readings stop when I stop?
- This ican be done by displaying all the readings on the Serial Monitor.
- Take the current code and start displaying gyro stuff if "acc" y-axis is at a threshold. Threshold for being in Ready state is `y>-.85` but that's just for turning the "system" on and off. The threshold for trying to look for Settled state is going to be different, and it's probably going to be based on being between `y>0.5` and `y>0.75` before it starts considering the gyroscope.
  - So, `if y>0.5` then _listen-for-gyro_ and then _wait-for-gyro-to-settle_ and then _begin-recording-gyro_ until a "gyro" type of threshold, when it stops recording. Maybe it's possible to record data points every 20millis, and stop after a specific number of data points. (yes)
  - And then after recording a few data points, it jumps into keyword spotting, meaning that it enables the microphone and waits to hear the word "yes", at which point makes the LED turn on or makes the phone beep.
  - Can test this by running a KWS test. The only code would be to turn on the microphone on the device, and wait to hear the key word. Like hears "yes" and LED turns on, then off after three seconds. This **test** can be isolated in our code. Go over one of the class lessons.
  - It looks like TinyML model could end up being KWS for this and also Gesture-type Model for gyro data. But now that I describe it like this, it doesn't sound so bad. After all, this is the same thing that happened in the class when image-spotting (face-ID) is ongoing, and when True, it switches over to KWS. **This** is the principle of **multi-tenancy**.

##### Steps:
- Finish App Development (**get the phone to beep**) and _finalize_ it, including video of success and simplifying code.
- Create simple KWS code.
  - **Turn the LED on and off with "yes" or "no".** Use Pete Warden's simple dataset _(refer to the class)_ so that I don't have to create my own, or use EdgeImpulse to create my own "yes"/"no" dataset, or do it myself as I did in the class.

#

#### Dog bark KWS example:
Sensor devices similar to the BLE Sense have been used to trigger audio to play from another device.
The dog barking example from YouTube (here) is one. 
So I need to use the matching code from that example and apply it for my purpose,
which is to get nRF Connect to trigger actions in my phone.
That microphone sensor created KWS model that triggered an app to play some prerecorded audio.
And I want my accelerometer to trigger my phone flashlight on/off, because it senses _Ready/Resting_ states.
- Found [**this**](https://youtu.be/v5hBjouFHQY) dog bark video about how the BLE Sense triggered other devices.
- Question is whether it's using BLE or some other connection. But it's a good example of KWS.
  - Wouldn't need BLE if listening device connects with wire to audio player!

#

**KWS idea.** Enable the microphone after the flashlight starts to function with the IMU.
Use the Warden Dataset to configure KWS to utilize "yes" and "no" to turn off and on the flashlight, using TinyML.
