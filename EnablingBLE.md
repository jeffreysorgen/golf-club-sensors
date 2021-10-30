- how to enable notify or indicate
- enable the descriptors

[Back](activity.md)

# What we've accomplished

1. Data is being sent from the device to nRF Connect
2. The device sends Ready/Resting depending on a threshold in the code
3. Accelerometer value is sent in the form of a **_hex_** (don't know if this matters yet)
4. I used UUIDs in their long form as constants [_(Notes about UUID)_](activity.md#notes-about-uuid)
5. I don't know if I could use a shorter form of UUID like "180C" or "300a"

#
##### Next:


### notify/indicate

We need to send notifications about a change of state between Ready and Resting.
We've syncronized the LED to turn on and off with this also.
As the code loops it sends its state every time through BLE. 
When the app reads "Ready" it is getting that information from the device constantly.
This is excessive. 


#


#### Identifying a state change and taking action
What I want is a way for my Android to recognize a state change coming from the **arduino**. 
- When the state goes from 0 to 1, I want the phone's flashlight to turn on. When it goes from 1 to 0, should turn off.
- More directly, state change into and out of Ready/Resting states. If `y < -.85` then turn on the flashlight on my phone!
- Construct code that will send a change of state notification when it happens, which can then be held in the nRF Connect app until the next update.
- Another way to look at this is by doing a check on **whether the states match** on the peripheral and client, and if it doesn't, to update the client, although it might require more communication between devices.
- There may be BLE-specific code that transmits _only_ when there's a state change, and could shorten this entirely, but for now I built it into this code. 
- This _pseudo code_ should transform into a function that updates the current readyState using incremental millis() or clock every 500ms. Then checks whether the state has changed, and if it did, it will send a Notify to BLE with `blenotify(notifyBit)`

##### State change: (pseudo code)
```
resting = state("Resting");
ready = state("Ready");
earlier = now;
now = update.state();        // returns "Ready" or "Resting"

if ( now !== earlier ) {     // if state has now changed
    if (now == resting) {    // and is now Resting
        beep(low);           // then beep low for new Resting state
        notifyBit = 0;
        }
    else if (now == ready) {
        beep(high);          // otherwise beep high for new Ready state
        notifyBit = 1;
        }
    blenotify(notifyBit);    // sends BIT to BLE "descriptor" (or something)
    }

else {
    pass;                    // now == earlier, so no state change
    }

earlier=now;                 // update earlier state with now state

```
#











Here's the _pseudo code_ to start going in the right direction. Figure out where this will fit into our system.










#
Here's the [FORUM for Arduino.](https://forum.arduino.cc/c/using-arduino/programming-questions/20)
- COPY HIS CODE

Go through entire process this user did implementing Notify/Indicate. **However this is developed, it's going to need CCCP, whatever that is.** 
- [(link to question) Set CCCD value](https://forum.arduino.cc/t/feature-request-option-to-set-cccd-value/919852)
- [(link to question) Notify/Indicate](https://forum.arduino.cc/t/notifications-and-indications-disabled-nrf-connect/915757)

He's discussing the nRF Connect functionality with Notify and Indicate. Also refers to video mentioned here about [BLE on Arduino](https://youtu.be/osneajf7Xkg) which shows some detail about Server/Client and characteristics
  - In this he mentions "BLE2902" _(figuring that out next)_ 
  - In nRF Connect shows up as "0x2902"

#
## Try the code

#

This should be coordinated like turning on and off the LED. 
The LED is literally turned on repeatedly through the loop. 
**We need to turn the LED on or off only when a change of state has been noticed.**
But use Notify/Indicate to get this to happen. 
Check the "2902" thing. `00` is off and `01` is on.
Obviously, rotating the y-axis is what makes this change happen.



#

### State Change info

**Important to enable Notify for Change of State**

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


#








```
update.state();               // returns "Ready" or "Resting"
    millis++1;
    if millis !== 1000 {      // adjust for latency
        pass;
        }
        else if ( y < -0.85 ) {
            return "Resting";
            }
        else if { 
            return "Ready"
            }
        }
      

```
#
#
#

#
#
#
(Trying to use multiple time increments but probably simpler is better intead.)

Here, I want to check if the current Ready state is True, and if it was also true for the last 2.5 seconds.
```
float y;                        // accelerometer y-axis
int millis;                     // integer number of millis up to 2500
bool readyState( a, b, c, d, e, f );  // save the past states as "Resting" (when first powered up)
bool stateChanged;               // state is consistent for 2.5 seconds

readyState ( a, b, c, d, e, f ) = False;  // all "Resting" until it isn't

millis ++1;                     // or use clock function
if millis == 500 {
    e = d; d = c; c = b; b = a; // shift array up by 500ms increment (need to use readyState(x))
    if (y < -0.85) {            // check for readyState
        a = False;              // readyState
        }
        else { 
        a = True;               // now the array has a new value
        }

        if ( a == c == e ) {            // for now, 800ms, 1200ms, 2000ms
          stateChanged = False;         // no change in state
          millis++1;                    // increment by 1ms, (or change to clock-check function)
          }
          else if ( b == c == e ) {     // a is different now
            stateChanged = True;
            millis = 0;
            }

    if stateChanged {
      SEND NOTIFY;
      }



create function update();       // create prototype function
  
if ( millis == 500 ) {          // has it been 500 millis since last update
  update();                     // 500 x 5 = 2500 millis
  }                             // else don't do update

```

#
#
#

#
#
#
