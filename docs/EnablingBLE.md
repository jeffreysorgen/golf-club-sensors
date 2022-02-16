[*[ Overview ]*](GolfSwingSensors.md/#golf-swing-sensors)
[*[ 1 The Accelerometer ]*](implementation.md/#the-accelerometer)
[*[ 2 Solve for Power ]*](implementation.md/#solve-for-power)
[*[ 3 Enabling BLE ]*](#step-three)
**[*[ 4 Collect Gyro Data ]*](#step-four-gyro)**
[*[ Step Four: Enable Smartphone Response ]*](AppDev.md/#step-four)
[*[ Step 4.5: Enable KWS ]*](KWS.md/#key-word-spotting)
[*[ Steps 5 & 6 ]*](activity.md/#steps-five-and-six)
[*[ Reference, etc. ]*](activity.md/#reference)
[_[ jump to new project ideas-> ]_](thoughtsandnotes.md/#other-projects)

##
##### Step Three:
(moved)


##
##### Step Four (gyro)
# Collecting Gyro Data

**Description:**

The Accelerometer has been used to determine whether a golf club is being used or has been put back in the golf bag, based upon its orientation.
When in play, it is able to begin recording the motion of the Gyroscope.






[Link to Gyro Steps](activity.md/#steps-five-and-six)


##
#
#
##
[*[ top ]*](GolfSwingSensors.md/#golf-swing-sensors)
[*[ 1 ]*](implementation.md/#the-accelerometer)
[*[ 2 ]*](implementation.md/#solve-for-power)
[*[ 3 (top) ]*](#step-three)
[*[ 4 data collection ]*](activity.md/#steps-five-and-six)
[_[ 0 Enable Smartphone Response (in progress) ]_](AppDev.md#step-four)
[*[ Reference ]*](activity.md/#reference)
[*[ 4.5 KWS ]*](KWS.md/#key-word-spotting)
[*[ 5 & 6 ]*](activity.md/#steps-five-and-six)
[_[ notes ]_](thoughtsandnotes.md/#other-projects)


##
### Code:

##
##### _BLE Hello World_ code is here:
[(back)](#the-hello-world-ble-sketch)

```
/*
  Arduino Nano 33 BLE Getting Started
  BLE peripheral with a simple Hello World greeting service that can be viewed
  on a mobile phone
  Adapted from Arduino BatteryMonitor example
*/

#include <ArduinoBLE.h>

static const char* greeting = "Hello World!";

BLEService greetingService("180C");  // User defined service

BLEStringCharacteristic greetingCharacteristic("2A56",  // standard 16-bit characteristic UUID
    BLERead, 13); // remote clients will only be able to read this

void setup() {
  Serial.begin(9600);    // initialize serial communication
  while (!Serial);

  pinMode(LED_BUILTIN, OUTPUT); // initialize the built-in LED pin

  if (!BLE.begin()) {   // initialize BLE
    Serial.println("starting BLE failed!");
    while (1);
  }

  BLE.setLocalName("Nano33BLE");  // Set name for connection
  BLE.setAdvertisedService(greetingService); // Advertise service
  greetingService.addCharacteristic(greetingCharacteristic); // Add characteristic to service
  BLE.addService(greetingService); // Add service
  greetingCharacteristic.setValue(greeting); // Set greeting string

  BLE.advertise();  // Start advertising
  Serial.print("Peripheral device MAC: ");
  Serial.println(BLE.address());
  Serial.println("Waiting for connections...");
}

void loop() {
  BLEDevice central = BLE.central();  // Wait for a BLE central to connect

  // if a central is connected to the peripheral:
  if (central) {
    Serial.print("Connected to central MAC: ");
    // print the central's BT address:
    Serial.println(central.address());
    // turn on the LED to indicate the connection:
    digitalWrite(LED_BUILTIN, HIGH);

    while (central.connected()){} // keep looping while connected
    
    // when the central disconnects, turn off the LED:
    digitalWrite(LED_BUILTIN, LOW);
    Serial.print("Disconnected from central MAC: ");
    Serial.println(central.address());
  }
}
```

##
##### All the _golf-swing-acc-ble_ code is here:
[(back)](#creating-the-new-code)

```
/*
 * Arduino LSM9DS1 
 * - Simple Accelerometer
 * golf-swing-acc
 * golf-swing-acc-ble
 */
 
// LIBRARIES
#include <ArduinoBLE.h>         // BLE library
#include <Arduino_LSM9DS1.h>    // IMU library

// CONSTANTS
static const char* imuUUID = "355d2b52-982c-4598-b9b4-c19156686e1a";
static const char* accUUID = "9e5982a7-9ef0-48e0-a167-8112ada5f184";
static const char* stateUUID = "9dc52af2-d585-4fb7-93a7-922b463239fe";

// INITIALIZE VARIABLES
/*
 * (example) String p, t, m; // Initalizing global variables for...
 */

// BLE SERVICE NAME (create service)
BLEService IMUService(imuUUID);

// BLE CHARACTERISTICS (create characteristics)
BLEFloatCharacteristic ble_accelerometer(accUUID, BLERead | BLENotify); // for the IMU service
BLEStringCharacteristic ble_state(stateUUID, BLERead | BLENotify, 10); // for the IMU service

// FUNCTION PROTOTYPE
/*
 * set up more functions here
 */

void setup() {
  // INITIALIZE THE SENSORS (and serial)
  IMU.begin();          // initialize IMU
  Serial.begin(9600);   // initialize serial comms
  //while (!Serial);    // comment this out

  // INITIALIZE THE DEVICE PINS
  pinMode(LED_BUILTIN, OUTPUT); // initialize the built-in LED pin
  
  // CHECK FOR FAILURE
  // BLE check
  if (!BLE.begin()) {
    Serial.println("starting BLE failed!");
    while (1); 
  }
  // IMU check
  if (!IMU.begin()) {
    Serial.println("Failed to initialize IMU!");
    while (1); 
  }
  
  // SET BLE NAME (create device name)
  BLE.setLocalName("Nano33BLESense");
  
  // ADVERTISE SERVICES
  BLE.setAdvertisedService(IMUService);
  
  // ADD CHARACTERISTICS TO BLE SERVICES
  IMUService.addCharacteristic(ble_accelerometer);
  IMUService.addCharacteristic(ble_state);
  
  // ADD SERVICES TO BLE STACK (add service to advertise)
  BLE.addService(IMUService);    // Add IMU Service
  
  // SET VALUES FOR STRINGS
  /*
   * setValue(x) stuff here
   */
  
  // START ADVERTISING (advertise all services)
  BLE.advertise();
  
} //s

void loop() {
  // Wait for a BLE central to connect
  BLEDevice central = BLE.central();

  // IMU activity
  float x, y, z;

  if (IMU.accelerationAvailable()) {
    IMU.readAcceleration(x, y, z);

    ble_accelerometer.writeValue(y);    // sends hex value

    // threshold is -1G
    if ( y > -.85 ) {
      
      // print to BLE
      ble_state.writeValue("Ready!");
      
      // engage the LED
      digitalWrite(LED_BUILTIN, HIGH);
      
      // print to Serial port
      Serial.print("Ready!");
      Serial.print('\t');
      Serial.print("X = ");
      Serial.print(x);
      Serial.print('\t');
      Serial.print("Y = ");
      Serial.print(y);
      Serial.print('\t');
      Serial.print("Z = ");
      Serial.println(z);
      }
    
    else { 
      
      // print to BLE
      ble_state.writeValue("Resting!");
      
      // engage the LED
      digitalWrite(LED_BUILTIN, LOW);
      
      // print to Serial port
      Serial.print("One second delay...");
      Serial.print('\t');
      Serial.print("Y = ");
      Serial.println(y);
      delay(1000); // one second delay
      }
    }
  } //v
```

##

[*[ top ]*](GolfSwingSensors.md/#golf-swing-sensors)
[*[ 1 ]*](implementation.md/#the-accelerometer)
[*[ 2 ]*](implementation.md/#solve-for-power)
[*[ 3 ]*](#step-three)
**[_Next: [ 4 ]_](AppDev.md/#step-four)**
[*[ Reference ]*](activity.md/#reference)
[*[ KWS ]*](KWS.md/#key-word-spotting)
[*[ 5 & 6 ]*](activity.md/#steps-five-and-six)
[_[ notes ]_](thoughtsandnotes.md/#other-projects)

