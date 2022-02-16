# Completed
##### Completed:

_skip to
2021
[\[Sept\]](#2021-sept)
[\[Oct\]](#2021-oct)
[\[Nov\]](#2021-nov)
2022
[\[Feb\]](#2021-nov)_

##### _2021 Sept_ 

1. **Git** Practicing with Repositories, Branches, Commits, Pull Requests, and Merges with [help from this Guide](https://guides.github.com/activities/hello-world/)
2. **Markdown** Learned to use [Markdown from this guide](https://guides.github.com/features/mastering-markdown/)
3. **TinyML** Took course for TinyML from EdX ([complete](https://credentials.edx.org/credentials/4601700d748c4de0b3a57315ff04831c/))
4. **activity.md** Describes my next steps, ideas, and this Completed list (now is a [to-do list](activity.md))
5. **golf-swing-idea.md** Defines the parts of my golf swing idea (incorporated into README and deleted)
6. **GitHub Pages** Added using [this guide](https://guides.github.com/features/pages/) and is accessible _here_ (ongoing)
7. Changed _hello-world_ (this) repository to PRIVATE, so it now isn't publicly available on GitHub Pages either (Now renamed to _golf-swing-sensors_ and **is** public. GHPages not enabled.)
8. Installed **GitHub Desktop**, and added [_ArduinoBLE-to-Android_](https://github.com/jeffreysorgen/ArduinoBLE-to-Android) repository (now may not be needed)
9. Defined Golf Swing Data Flow, [_below_](implementation.md#flow-for-golf-swing-sensors)
10. Created _new-readme.md_, _(then README.md, then [GolfSwingSensors.md](GolfSwingSensors.md))_, added implementation.md (this) and made [activity.md](activity.md) into a TO-DO list. (Now deleted the old readme-old.md.)
11. Renamed (this) repository: _hello-world_ is now _golf-swing-sensors_, and updated README.md _(now content is in [GolfSwingSensors.md](GolfSwingSensors.md#golf-swing-sensors)_
12. [Implementation Part One:](implementation.md#the-accelerometer) **Set up Accelerometer**

##### _2021 Oct_

13. Determined that the Magnetometer is _not_ going to be used for Ready/Resting orientation. _Gained understanding._ Learned that Accelerometer is going to be used, and magnetometer is unnecessary for the project.
14. Determined that it is _better_ to use `millis()` rather than `delay()` in some cases.
15. Figured out how to set up Ready/Resting states in Arduino Sketch, and display in Monitor.
16. LEARNED that it's easy to mess up in GitHub desktop, so _be careful_
17. **Electronics** Learned cursory principles of electronics from __LinkedIn Learning__.
18. **3D Printing** Learned Basics of 3D Printing from __LinkedIn Learning__.
19. **Arduino sketch** created for Accelerometer - _golf-swing-acc_ ([here](implementation.md#updating-the-arduino-nano-33-ble)) and kept it **simple**. 
20. **GitHub Markdown** Learned styling for tables, images and code block
21. Added code block and image from Monitor to this documentation. 
22. **Accelerometer** Completed documentation, and it's time to move on to [**adding BLE**](EnablingBLE.md/#enabling-ble) to the system!
23. **Add BLE** Successfully [copied the process](EnablingBLE.md/#try-the-ble-example-sketch) of a YouTuber to add BLE services on the device.
24. **Power** Documented several potential development and prototyping [**battery solutions**](implementation.md#solve-for-power)
25. Generated _pseudo code_ for state changes. (Was hepful, now deleted.)
26. **New Code** Created [_golf-swing-imu-ble_](EnablingBLE.md#all-the-golf-swing-acc-ble-code-is-here) which communicates via BLE to a smartphone
27. **Editing** Consolidated the documentation in preparation for moving to **Step Four: [the SDK!](EnablingBLE.md#step-four)**
28. **Posted to Reddit!** [HERE](https://www.reddit.com/r/arduino/comments/qja9y6/nano33blesense_project_first_step_solve_for_power/)

##### _2021 Nov_

29. **CODE** Solved the sensor "bounce" problem with accelerometer and enabled READY/RESTING states. _(golf-swing-imu-ble-statechange)_
30. **Bluetooth** BLE has been properly enabled to communicate with smartphone, time to dig into the SDK
31. **KWS.md** Page for Key Word Spotting step has been set up, [_here_](KWS.md), and step is in progress.
32. Modified the _README.md_ down to one sentence, and then created [_GolfSwingSensors.md_](GolfSwingSensors.md) for the rest.
33. Determined that Android Studio will be the starting point to develop the Client app for the project (step four)
34. **Android Studio** Completed "Android Studio Essential Training 2020" in **LinkedIn Learning**
35. Started working with **[Kotlin](https://kotlinlang.org/)** and **Android Studio**
36. Bought domain _"motion.golf"_
37. **App Development** Installed Android Studio
38. _"Hello World"_ app created for Android
39. Set up **Virtual environment**
40. Set up **Git** in the virtual environment
41. Set up **Jupyter Notebooks** to function in the virtual environment

##### _2022 Feb_

41. Halted App Development in favor of **Data Collection**
42. Consolidate documentation


##### Learning about this:
- Gyroscope
- Memory allocation
- Arduino - **Arrays**
- IMU sensors, BLE, C++, data collection
- GitHub Desktop, possibly GitBASH, KiCad, LTspice, OSHPARK
