##### Digging into BLE
[Back](activity.md)

# What we've accomplished

1. Data is being sent from the device to nRF Connect
2. The device sends Ready/Resting depending on a threshold in the code
3. Accelerometer value is sent in the form of a **_hex_** (don't know if this matters yet)
4. I used UUIDs in their long form as constants [_(Notes about UUID)_](activity.md#notes-about-uuid)
5. I don't know if I could use a shorter form of UUID like "180C" or "300a"

# State change

##### Important future change:
#### Notify on change of state

One of the future modifications needs to be utilizing the BLE code that features **state change only** notifications, so that nRF only receives one-time signal that the state has changed between Ready and Resting, rather than as it is now, which always prints its state to BLE. 

**Now that state changes can be sent to the smartphone, try to turn its flashlight on/off with the signal!**
What we want to do for this project is to read information from the sensor and then get the phone app to act upon the capabilities of the phone, such as turning on a flashight or beeping. 
While the flashlight functionality won't be used in the end, that solution is crucial for when we're trying to get the phone to chirp good/bad golf swings. 
- There is a difference between constantly notifying about the state and simply notifying about a state change.
- Notifying only about a state change will be helpful to eliminate unnecessary BLE communication. 
- Checking a state change can happen less frequently than the device baud rate, so we don't get bounces of the states due to natural movement. 
  - For example, during its transition to a new state the LED lit very briefly, flashing the previous state of the LED. It looked like a bounce.
- [LINK to more here](#state-change-reference-in-here) 
- [Another link to state change](#state-change)

#
### notify/indicate

Link to Arduino forum about [notify/indicate](https://forum.arduino.cc/t/notifications-and-indications-disabled-nrf-connect/915757)
```
const uint8_t notificationOn[] = {0x1, 0x0};
pRemoteCharacteristic->getDescriptor(BLEUUID((uint16_t)0x2902))->writeValue((uint8_t*)notificationOn, 2, true);
```
He's pointing out the nRF Connect functionality with Notify and Indicate. Refers to video mentioned in [resource section](#resources)













#
#
#

#
#
#
