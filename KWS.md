[(activity.md)](activity.md)
[(EnablingBLE.md)](EnablingBLE.md)

##### Step 4.5
# Enabling KWS
Now that we're able to get a signal from the Nano33BLESense 
to prompt the Android smartphone to turn its flashlight on and off, 
let's now reprogram the device utilizing Key Word Spotting to do the same thing 
just by speaking into the microphone "Yes" and "No".

1. Enable the microphone
2. Test simple code that the mic works, usually from the Arduino example files.
3. (big step here: KWS) Get it to understand the words "Yes" and "No" versus all other words. _Only need the word "Yes" in the end, so will go through this process again later._

. . . . . . . . .

#### Key Word Spotting
- Create simple KWS code.
  - **Turn the LED on and off with "yes" or "no".** 
  - Use Pete Warden's simple dataset _(refer to the class)_ so that I don't have to create my own, or use EdgeImpulse to create
- Use the Warden Dataset to configure KWS to utilize "yes" and "no" to turn off and on the flashlight, using TinyML.


**Step 4 Development (Enable Smartphone Response):**
- App Development: make phone beep for Ready/Resting state
- Finish App Development (**get the phone to beep**) and _finalize_ it, including video of success and simplifying code.

**Step 5 Development (get the gyro going):**
- Study how gyro works. What do the readings look like? When I wave around the device, are the readings moving around? Do the readings stop when I stop?
- This can be done by displaying all the readings on the Serial Monitor.
- Take the current code and start displaying gyro stuff if "acc" y-axis is at a threshold. Threshold for being in Ready state is `y>-.85` but that's just for turning the "system" on and off. The threshold for trying to look for Settled state is going to be different, and it's probably going to be based on being between `y>0.5` and `y>0.75` before it starts considering the gyroscope.
  - So, `if y>0.5` then _listen-for-gyro_ and then _wait-for-gyro-to-settle_ and then _begin-recording-gyro_ until a "gyro" type of threshold, when it stops recording. Maybe it's possible to record data points every 20millis, and stop after a specific number of data points. (yes)

**Step 6 Development (collect gyro data):**
- When in Ready state, turn on Gyro, finish Gyro motion, _then listen for key word_.
- Enable gyro when in Ready state, and start recording information when gyro readings "SETTLE"


**Multi-tenancy ML:**
- Start using **TensorFlow Micro**.
- Can this be applied after app development stage? After I get the phone to beep when the sensor reaches threshold, can it listen for key word and act on the sensor output to make the smartphone turn on the flashlight? Listen for "on"/"off" or "yes"/"no" and then turn the LED on or off?
- After recording a few data points, it jumps into keyword spotting, meaning that it enables the microphone and waits to hear the word "yes", at which point makes the LED turn on or makes the phone beep.
- Can test this by running a KWS test. The only code would be to turn on the microphone on the device, and wait to hear the key word. Like hears "yes" and LED turns on, then off after three seconds. This **test** can be isolated in our code. Go over one of the class lessons.
- It looks like TinyML model could end up being KWS for this and also Gesture-type Model for gyro data. But now that I describe it like this, it doesn't sound so bad. After all, this is the same thing that happened in the class when image-spotting (face-ID) is ongoing, and when True, it switches over to KWS. **This** is the principle of **multi-tenancy**.
 my own "yes"/"no" dataset, or do it myself as I did in the class.

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

