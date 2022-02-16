Reference.md
#
**This page is now "Reference, etc."**
- [Reference](#reference)
- [UUID Info](#uuid-info)
- [Magic Wand Info](#magic-wand)
- [( <-- Enable Smartphone Response )](EnablingBLE.md/#step-four)
- [Enabling the gyro](#steps-five-and-six)
- [Recording the data](#steps-five-and-six) 

##
# Reference:

- Here's the okdo.com example, including [_BLE Hello World_](#the-ble-hello-world-sketch): [**getting started** from *okdo.com*](https://www.okdo.com/getting-started/get-started-with-arduino-nano-33-ble/#h-1-configure-ide-toc)
- **Hackster** tutorial: Here's a helpful [IMU and BLE](https://www.hackster.io/gov/imu-to-you-ae53e1) tutorial from hackster.io
- Wiki about [C data types](https://en.wikipedia.org/wiki/C_data_types#stdint.h)
- Helpful from Argenox:
  - [**Argenox website**](https://www.argenox.com/library/bluetooth-low-energy/ble-advertising-primer/) is a good place to READ about BLE
  - Here's the [Bluetooth Low Energy Library](https://www.argenox.com/library/bluetooth-low-energy/)
  - Here is a link for [BLE and batteries](https://www.argenox.com/library/bluetooth-low-energy/powering-ble-batt/)
- **Arduino** resources:
  - Arduino's reference for BLE](#arduinos-reference-for-ble) _(double check this link)_
  - Go through all the _ArduinoBLE_ sketches **in the Examples folder in the IDE**
  - Also use the [**Arduino guide for NANO33BLESense**](https://www.arduino.cc/en/Guide/NANO33BLESense) for reference
  - A complete [reference](https://www.arduino.cc/reference/en/)
  - And one specifically for [BLE](https://www.arduino.cc/reference/en/libraries/arduinoble/)
- **Nordic Semi** resources:
  - Helpful [**beginners tutorial**](https://devzone.nordicsemi.com/nordic/short-range-guides/b/bluetooth-low-energy/posts/ble-advertising-a-beginners-tutorial) from Nordic Semi. 
  - And another [**here.**](https://devzone.nordicsemi.com/nordic/short-range-guides/b/bluetooth-low-energy/posts/bluetooth-smart-and-the-nordics-softdevices-part-1)
#### _Reader and Sender:_ 
- Definition from [ArduinoBLE Reference](https://www.arduino.cc/en/Reference/ArduinoBLE)
- Think of this as _Sender_ and _Reader_. 
ArduinoBLESense device is the _sender_ (Peripheral). 
When a reading changes, the nRF Connect app is going to be the _reader_ (Client).
The model BLE uses is known as a "publish-and-subscribe" model.
#### _Coding Tips:_
- The all-inclusive Arduino file will be saved as _golf-sensors.ino_ when more sensors are involved.
  - Multiple "h" file can probably be included, to split off logically (see: [_magic wand_](#magic-wand) example) from the _.ino_ file.
- For images, this is helpful: resizing and centering with `<p align="center"><img src="http://some_place.com/image.png" /></p>`
- Change LED from one state to the other: `digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));`

##
## UUID Info:

- [Online UUID Generator](https://www.uuidgenerator.net/) created the [unique UUIDs](#15-unique-v4uuids)
- Need to have specific UUIDs for each IMU param
- Can't find a specific standard UUID for x,y,z on the Accelerometer
  - check _GATT Angulars_ note, [**here**](#gatt-angulars) 
  - what does the **raw** IMU data look like?
- Here's some GATT information [(_LINK_)](https://www.oreilly.com/library/view/getting-started-with/9781491900550/ch04.html) from O'Reilly (2014), and its repository, [(_here._)](https://github.com/microbuilder/IntroToBLE)
- Check this hackster.io post, [**here**](https://www.hackster.io/gov/imu-to-you-ae53e1), again
- Changed to 16-bit in the code, "FFE0", etc.

##### 15 unique v4UUIDs:
```
355d2b52-982c-4598-b9b4-c19156686e1a    // for imuUUID (ffe0)
9e5982a7-9ef0-48e0-a167-8112ada5f184    // for accUUID (ffe1)
9dc52af2-d585-4fb7-93a7-922b463239fe    // for stateUUID (ffe2)
8564aabe-417c-4fe4-8a40-543ea08079f4    
3e8c97c5-6ae5-444f-b56e-20a741e7bf99
f2024cef-dae8-4db7-bddb-76c696cdc115
62237f9d-7652-442a-a36a-0a68d96bd617
f22e0e5c-9636-4a83-9eaa-ba309101c4b6
88abdb86-22fb-4b2f-8d35-53bb942625b7
0a374697-2847-4d15-b7b2-b89281022f65
b60136d1-e8c5-4042-82e8-a0f6fcd4f6d4
989ae3c5-6e5a-4868-96c7-011511e880b2
d49b4462-2bfd-4d92-8103-88ed9429e662
52f6c067-db43-4e53-893b-d7d98406901b
fa94204d-dc71-4585-aa63-98b8133c5266
```

**More UUID-Specific info:**

##### GATT angulars:
**Interesting:** There are two GATT units, 0x2743 and 0x2744, which are _angular velocity (radian per second)_ and _angular acceleration (radian per second squared)_, respectively. Don't know whether I'd be able to use this. It's related to centripetal force. _But if I use this measure, it will appear in the MCU code and not transmitted via BLE._

##### UUID BLE specification:
_I read that this was important for BLE:_ UUID for BLE: _"XXXXXXXX-0000-1000-8000-00805F9B34FB"_ _(Look this up to find standard BLE list)_

##
## magic wand
- **LEARN** 
[from the course](https://learning.edx.org/course/course-v1:HarvardX+TinyML3+1T2021/block-v1:HarvardX+TinyML3+1T2021+type@sequential+block@e355a78c0dcd49b6acbeeaf8f7492859/block-v1:HarvardX+TinyML3+1T2021+type@vertical+block@6e2f8e18dd814e63ad68f60e380b6633)
about the _magic-wand_ sketch to see how the DATA is recorded there and what gets transmitted to the Serial Monitor, and then how that data displays on the Monitor from that data. What converts that data to the 'readable' visualization of the motion?
[**This** is the link to the course data collection browser app (use Chrome)](https://tinyml.seas.harvard.edu/magic_wand/).
- **TEST the motion of the gyro/acc.** Can this motion show up on the Plotter or Monitor? What does this motion look like for gyro/acc individually? Is it helpful to sample the data more slowly for better visualization?
- HOW does this data get recorded into a data point? We recorded a data set for the Exercise. So _how was that collected?_
- **_Can this activity happen LOCALLY?_** Because the exercise actually resided on _tinymlx.io_ or something. And that's where all the data got generated.
- **How much reliance upon external websites is necessary? Why not ALL local?**
