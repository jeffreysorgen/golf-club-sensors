- how to use the UUID being send to _nrfconnect_
- how to enable notify or indicate
    - what setting on the peripheral is used to send on/off to the client?

[Back](activity.md#summary-so-far)

# What we've accomplished

1. Data is being sent from the device to nRF Connect
2. The device sends Ready/Resting to the app depending on a threshold in the code
3. Accelerometer value is sent in the form of a **_hex_** (don't know if this makes a difference in our project)
4. We used UUIDs in their long form as constants [_(Notes about UUID)_](activity.md#uuid-info)
5. Then started to use a shorter, 16-bit form of UUID, like "ffe0" and "ffe1"

#

Right now, information is being sent through BLE every time the code loops.
We need to send notifications about a change of state to the Client (nRF Connect) when the peripheral changes its state from Ready to Resting or back.
When the app reads "Ready!" or "Resting!" it is receiving 6 or 8 bytes of information from the device constantly, which is excessive. 

### Peripheral-side code is done

**The new code now has this function.** (_golf-swing-acc-ble-statechange_) 
We needed to modify the code so that it will only send data via BLE when necessary, when the state switches between Ready and Resting. 
This reduces the BLE communication (which is the most energy-hungry part of this project) down to one single instance: _a characteristic change_ (state change). 
We substituted using entire words, "Resting" and "Ready", and instead used _boolean 1/0_ to do the same thing.
It looks like this in the monitor, and sends BLE data only at the point when the words "State change to" show up.

##### State changes by tilting on the y-axis
  <p align="center"><img src="images/stateshanges.gif"  width="90%"></p>

#
The code:
- Added a timing functionality so it checks for new state, and accommodates for any unintentional bounces.
- Added logic to the code so that Resting is 1 and Ready is 0
- _did other things here but I don't know where to go with this right now_

And now:
- _Most likely, need to just figure out what to do in nrfconnect, and **act upon the UUID changing from 0 to 1**_
#
#
##### done to here
#
So we need to enable Notify functionality (or Indicate) so that we can send the data once and be done until the state changes again.

**Updating a characteristic.** 
When Y-axis, `y < -0.85`, changes from True to False or back, this is the moment to send BLE data and nothing else, to save on BLE energy. _We now need to adopt this energy-saving code._

**Notify or Indicate.** 
(Here's something from [ArduinoBLE Reference](https://www.arduino.cc/en/Reference/ArduinoBLE))
Think of this as _Sender_ and _Reader_. 
ArduinoBLESense device is the _sender_, or Peripheral. 
When a reading changes, the nRF Connect app is going to be the _reader_, or Client.
The model BLE uses is known as a "publish-and-subscribe" model.

#

#### Responding to a state change
We're going to try to enable one element of the BLE functionality which will send a notification only when the state changes.
The code will send a change of state notification when it happens, which can then be held in the nRF Connect app until the next update.

This **_pseudo code_** should transform into a function that updates the current readyState every half-second, using incremental millis() or clock every 500ms. 
In the loop it will check whether the state has changed, and if it did, it will send a Notify to BLE with `blenotify(notifyBit)`. 
_Going to need to learn to code a function now._ (But are we?)

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
_Going to need to learn to code a function now._

We're going to modify _golf-swing-acc-ble_ (now **_golf-swing-acc-ble-NOTIFY_**), and include the code that will notify the Client of a state change. 
We can see this in "Client Characteristic Configuration" or UUID "0x2902".

- First, disengage the loop to print to the Serial port so we can just see whether our function was called.
- Maybe print "Ready" just using _print_ so it doesn't scroll down
- And for testing, put a delay() in to only loop a few times a second. 
- Call the function every half second, so use `if millis(500)`


#

Need to simplify the code.
What is the purpose?
Club is in Resting state, so halt all things for one second.
When it's in resting state, there is a delay before checking for its state again.

First things first.
Club is in the bag, getting rattled around. 
Resting state, but pops into Ready state sometimes. 
If it pops into Ready state, then we should verify that it's in Ready state
and not just bouncing around.
Resting state has to exist for most of two seconds.
(Or, Ready state needs to coordinate with the GYRO right at that moment.)
But checking 'nowstate' and 'earlier' isn't enough.
Need to compare 'nowstate' to 'earlier' and 'muchearlier' also.
Basically, `if nowstate == laststate == earlier == muchearlier {no state change}`.
If nowstate is Resting then delay two seconds to check again.

```
setup loop() {
    nowstate = laststate = earlier = muchearlier = false;
void loop() {
    read_the_y_axis();
    if (y > -.85) {
        readystate = true;
        do stuff;
        do gyro stuff etc;
        }
        else {
            Serial.print("Resting...");
            // triple-check if still Resting
            readystate = false;
            muchearlier = earlier;
            earlier = laststate;
            laststate = nowstate;
            if (nowstate == muchearlier) {
                nowstate = laststate = earlier = muchearlier = false; // was just a bounce
                nostatechange = true; // still Resting
                delay(2000); // because resting
                }
if !readystate {
    Serial.print("Resting ");
    Serial.println(y);
    }
    else {
        pass; // Serial.print("Ready!");
    }
 
    


```



#

#

#

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

#
- There may be BLE-specific code that transmits _only_ when there's a state change, and could shorten this entirely, but for now I built it into this code. Is there a way for the client to ask the peripheral whether the state has changed? Maybe. But how frequently and how much power consumption. Of course, the peripheral could ignore requests for update as well. Unless there's another way to think about this, I don't think this matters much. No savings of effort or energy.
- Another way to look at this is by doing a check on **whether the states match** on the peripheral and client, and if it doesn't, to update the client, although it might require more communication between devices.


#
Flashlight:
- When the state goes from 0 to 1, I want the phone's flashlight to turn on. When it goes from 1 to 0, should turn off.
- More directly, state change into and out of Ready/Resting states. If `y < -.85` then turn on the flashlight on my phone!

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
