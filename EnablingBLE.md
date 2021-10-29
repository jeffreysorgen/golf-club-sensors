##### Digging into BLE
[Back](activity.md)

# What we've accomplished

1. Data is being sent from the device to nRF Connect
2. The device sends Ready/Resting depending on a threshold in the code
3. Accelerometer value is sent in the form of a **_hex_** (don't know if this matters yet)
4. I used UUIDs in their long form as constants [_(Notes about UUID)_](activity.md#notes-about-uuid)
5. I don't know if I could use a shorter form of UUID like "180C" or "300a"

## State change

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
- [LINK to more here](activity.md#state-change-reference-in-here) 
- [Another link to state change](activity.md#state-change)

#
### notify/indicate

Link to Arduino forum about [notify/indicate](https://forum.arduino.cc/t/notifications-and-indications-disabled-nrf-connect/915757)
```
const uint8_t notificationOn[] = {0x1, 0x0};
pRemoteCharacteristic->getDescriptor(BLEUUID((uint16_t)0x2902))->writeValue((uint8_t*)notificationOn, 2, true);
```
He's pointing out the nRF Connect functionality with Notify and Indicate. Refers to video mentioned here:
- Here's a YouTube video ( [*Bluetooth BLE on ESP32 works! Tutorial for Arduino IDE*](https://youtu.be/osneajf7Xkg) ) that shows some detail about Server/Client and characteristics
  - and in which he mentions "BLE2902" but I can't find usage for it yet. But it showed up on nRF Connect "0x2902"

#

#

#

## State Change Info
##### Identifying a state change and taking action
What I want is a way for my Android to recognize a state change coming from the **arduino**. 
- When the state goes from 0 to 1, I want the phone's flashlight to turn on. When it goes from 1 to 0, should turn off.
- More directly, state change into and out of Ready/Resting states. If `y < -.85` then turn on the flashlight on my phone!
- There may be BLE-specific code that transmits _only_ when there's a state change, and could shorten this entirely
##### State change: (pseudo code)
```
resting = state("Resting");
ready = state("Ready");
now = update.state();         // returns "Ready" or "Resting"

if ( now !== earlier ) {      // if state has now changed
    if (now == resting) {     // and is now Resting
        beep(low);            // then beep low for new Resting state
        }
    else {
        beep(high);           // otherwise beep high for new Ready state
        }
    earlier=now;              // update earlier state with now state
    }
    pass;                     // now == earlier, so no state change
```
#









#
#
#

#
#
#
