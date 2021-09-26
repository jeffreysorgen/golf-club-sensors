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
