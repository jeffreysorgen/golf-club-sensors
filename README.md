## Golf Swing Sensors
This repository consists of code and any related descriptive narrative about the implementation of my Golf Swing Sensors project. I'm developing this on Arduino Nano 33 BLE Sense, employing TinyML (tfmicro) for inference when needed, and BLE (Bluetooth low energy) for communication between the Nano and a smartphone.

### Background
As an amateur golfer, it's important to get real-time feedback on the condition of my golf swing. I need a way to instantly know whether I am swinging my club correctly prior to hitting the ball, or if I've got it right. For me, the psychology of approaching the ball goes in two directions. One, I am confident and relaxed, or two, my mind is racing and I need to quiet it down.  

What primarily causes me to think too much? I've just flubbed the last shot and I'm trying to get over it. I need to eliminate 100% of the flubs in my game. I would rather concentrate on my aim and club selection decisions, rather than on the goofy swing I am trying to avoid.

Imagine approaching your ball with your club in hand, and taking a few practice swings. In your head, a lot of things are going on based on what you've heard recently from your coach, maybe have seen in coaching videos, or what you heard moments ago from your excellent golf partner.

If you're like me trying to remember what you saw on YouTube, then it's hard to recall the information in a way that's helpful at the very moment you step up to the ball. And your game will be long over when you finally get back to studying how well you did by reflecting on your score as you search for new tips from your favorite sources.

**This project is going to help me with my swing by providing instant feedback when my practice swing is not where it needs to be. I'll instantly know the difference between a good swing and a bad one.** 
- With a good swing I'll hear a positive chirp from my smartphone, which I keep in my golf cart or bag. 
- With a not-so-good swing, I'll hear a sad chirp.

### Purpose of the Device:
There are _two_. The first is data collection, described here. Its architecture expects me to say 'yes' following a good swing. 

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
- Start by connecting the MCU to the smartphone
- Begin IMU void loop

Uses IMU:
- If Event Data exists, send data via BLE and clear buffer
- IMU waits for zero orientation on plane, 
  * then waits for motion to settle, 
  * then enables recording of swing motion (event loop) 
- record the motion event
  * store motion data
  * engage the KWS 

Uses KWS:
- KWS void loop listens for 'yes'
- after delay, records null label
- after 'yes' Keyword, records Yes label then returns to IMU input

#
**Next: [*[ Implementation: Accelerometer --> ]*](implementation.md#step-one)**
