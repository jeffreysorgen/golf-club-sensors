[Back](activity.md)

# Thoughts and Notes:

## Other Projects:
_(Jot down ideas for other projects here)_
- **LED glasses** which display "HIGHLND" across them (**RECEIVED!!**) Project: [_ledglasses_ respository (private)](https://github.com/jeffreysorgen/ledglasses)
- **Golf Bag Robot** Attach servos and a wheel to my golf bag, and make it use Image Recognition to follow me around on the golf course. Hold the flag up (or use red/green _COLOR_) and it moves forward, to the side and it goes that way, hold it down and it stops. _Good idea, but not for profit. Just fun._
  - _Color "flag" stick._ Colors on two ends of a stick. One end is Yellow, both sides. The other end is Green and Red, respectively, on opposite end of the stick. Holding the Green dot up and Yellow down, move forward. If Green is to the Side of the Yellow, then go that way! If Red dot is visible, then stop. If Yellow is above the Green, then straighten out and go fastest possible. Could be yellow triangle, green square, and red circle.
- Wind turbine ( Is the most popular product TinyML or IoT? )
- Continue recording _golf-swing-sensors_ progress in THIS repository


## Fourier
- I would like to convert MOTION recorded here and stored in JSON into a _Fourier Series_ because it seems that that would be a good way to create 3D time-series illustrations
- ([3B1B Fourier Series video here](https://youtu.be/r6sGWTCMz2k?t=1226)) The video describes 2 dimensions, but what I'm interested in calculating is in 3 dimensions.
- This formula has hyperparameters which I believe can be learned by a Neural Net. A golf swing consists of a time series of 3D points (say, 100 3-dimensional points per second), and can entirely be drawn with a **Fourier Series!**
- Technically, although virtually impossible to visualize as a human being, this will work in _**n-dimensions!**_ 

This Fourier part might be the most important and useful part of the project.
If I can find a way to collect instances of 4 dimensional data, I can record the motion of anything a sensor is attached to in 3D space with velocity. **There will be math calculations involved. Start with 2D. Break down the coefficients into a list. But first, figure out how to record the data.**

The reason we want to use Fourier Series rather than just data points, is that it can use quantization. Is the memory footprint smaller using data points, or using float? Fourier Series needs to use Float because it's using complex numbers. **Just use data points**

### This means?
- that a _multi-dimensional_ line can be generated, against which comparisons can be made for a VERY QUICK INFERENCE!
- To illustrate, imagine a drone on a 3D path, plus time, and add 2 more dimensions (parameters) such as temperature and pressure. Now it's a 6-dimensional "line" (shape) against which a model can make a quick inference!
##
The above might describe some complete science fiction. I  may be thinking about this in the wrong way. After a very superficial review of Fourier Transform, I think that "3D" aspects are asking too much. I really don't think this is what I am looking for. _(Oh?)_
- It might be though. What if the complex calculations taught about FT are in fact learnable by just including x,y,z and time in a machine learning model.
  - _A good swing has these coordinates across this timeline, so yes, it's a good swing by inference._

### Or it's possible that...
- all I really need is something that we learned about when we were analyzing spectrograms from AUDIO samples, and transforming them using **FFT (Fast Fourier Transform)** so maybe this can be applied to my problem, where there is a singular path (2D), but with one additional corresponding parameter.
- It's actually much like combining multiple sensors, isn't it?!! After all, an IMU with 9DOF actually has 9 readings, so of course being a mere human being, I couldn't see that we would just utilze what ML already knows how to do.

Think about how FFT works and what it works on: audio input in the form of a sound wave. Many data points, parsed out to become a list of **data points.** So rather than thinking that what I'm doing needs to **become** a Fourier Series, we simply need to CREATE A DATABASE OF DATA POINTS, which clearly will be small footprint, in the order of 1k rather than 1M. If one **swing** contains one hundred points of four dimensions, then 4x100 = 400 characters. TINY.

I'M OVERTHINKING THIS IF I'M USING FOURIER SERIES! **JUST USE A DATABASE, JUST COLLECT DATA.**

Even if I wanted to recreate the golf swing in an animation, I would only need these data points. The 4th dimension would be set to a clock, and the other 3 are axes.

### So what am I looking for, anyway? (I'm exhausted.)
- I think all I'm actually looking for is a real-time 3d representation connecting all the data points of the golf swing. The ML can figure out all the other stuff on its own when I send it in the right direction.
- **is FFT useful here?** is this going to be like converting audio spectrograms?
#
##### 21oct22
If there were a way to CREATE a tiny microcontroller board which contains an IMU programmed the way I want to use it with the golf swing, then I can imagine that it might be useful in a wide variety of purposes.
These purposes would center around collecting data from the IMU as it moves very swiftly through the air, or even in a very small area.
I suppose it would be a bit like anomaly detection in the realm of machinery vibration.
But this is movement.
I'm trying to say that my knowledge from completing this golf club sensors project could extend into other areas.
I don't know... I'm just rambling. But I intended to record some thoughts here, about having a tiny MCU with BLE module and associated memory chips involved, and could broadcast its data (I'm trying to develop this now) to receiving points along the way. (I'm imagining a subway train for some reason, and dropping off data along its path, becoming refreshed so it can collect more data.)
This seems like just a data collection thing. It is. But I'd like to include ML inference AT THE SAME TIME. So, I would be able to collect information, apply a label to it based on its ML Categorization inference.

In the golf swing, data is collected one swing at a time. This is what I was trying to describe. (But it went to a subway train instead.) There must be good ideas about how this motion capture can be utilized in this way.

My project is designed to first collect data by labeling each swing myself, and expanding that dataset to having other people make the swing and label the swing good or not.
I am trying to simplify this process so much that it is possible for a high school golf coach to collect data.
And then I want to have that specific data incorporated into the available dataset, and then repeated, so that I can eventually collect the worlds first and largest dataset of good golf swings. 

**I know that's ambitious,** but it's a hard project and I want to get it done, for the experience.
#
Speaking about using ML to respond with an inference: I imagine a physically separate MCU which receives this data from MCU#1 as a data point. MCU1 generates the data, MCU2 makes an inference about it and sends that combination to MCU3 which collects that data. I am imagining MCU3 is the piece which stores the data, and then sends it via BLE to a receiving device which has more storage like IoT device, which can in turn send to the cloud.

Conceptually, this is still **the subway analogy.** But makes it easier to think about. Maybe this analogy is not a bad one while I create this. The golf swing is the subway route, club head is the subway car, device is attached to the surface of the subway car, and one swing is analogous to the route between stops with all its curves and speeds.

Using this analogy, it really seems like it's been done before: _self-driving cars,_ and that whole system. What are they using? Do they rely on GPS too much? Yes, in comparison, I think they do. But they do speed, distance, turns, bumps, motion, etc. And they collect data, and they make inferences along the way.

However, I am removing all the horsepower. I am greatly simplifying what's been done with giant compute power. If I can reduce this information into data points with a _shape_ under 10 (xyz axes, time stamp, plus other related params) then I'm going to be able to utilize MCU rather than the larger devices. And if I'm able to use this in the form of **Fourier Series** then it might be able to be **reduced further in the ML model.** Yes, I'm back to that again! I know there's complicated math. And anything beyond 2D is hard to describe. I don't yet understand the correlation with the FFT (fast fourier transform used for converting audio spectragrams) but I feel strongly that this is _important._
##
#
#
##
**Note for repository description.** This repository is the narrative of my own progress in development, from start to... to wherever I am now. This can be a beginners guide because it's all new to me, and I am including all the stuff that I need to always remember.
#
IDEA: (I didn't know where to put this)
Use the microphone on the BLE device to turn on and off the flashlight on my phone.
Do this right after I finish attaching the flashlight to the y-axis tilt.
#
Data collection. How can Edgeimpulse be used for data collection? Prof said I could use it somehow but there's no indication about how they could collect data. They like to acquire it though. I send it up, and then they do something with it. But that doesn't make easier the data collection.
#
Bought motion.golf domain 21nov9
#
There are a lot of new data science people out there. I need to capitalize on those fresh skills and provide a place for those skills to be useful. 
I need to get my ML code/model perfected and I'm giong to need some of these pros to help do that.
I obviously can't afford to pay a pro. But I can offer a dataset to use, with goals to attain.
As I do some of this on my own, I'll be able to understand where my weaknesses are and then ask for specific milestones to be accomplished well by others.
I have always thought that **Kaggle** would be a good place to start. But there are probably other ways too.
#
The Nano33BLESense has RGB LED. I want the app to trigger the BLUE led when it receives data from the Android, such as a button push or a toggle.
##
#
**RFID**

Having a small form factor, the device attaches velcro to the club head.
Velcro strip on every club head includes an RFID "strip" which makes it possible for the device to recognize which golf club it's attached to.
#
Think about using a raspberry pi as a "home" unit, which can reside in the golf cart or bag.
Micro SD card is available in the raspberry pi.
It can deliver power to the arduino (for prototyping)
#
##
What if I'm decomposing this problem incorrectly?

Why not get everything working without the app and then when that's done try putting the app together?

It seems that the application is an ambiguous concept for me, because the flow isn't precisely clear.
I have wanted the MVP, the hello world for my project that included an app with which to record Y/N into the database.

But I do not have a database.

Alter this project using all-wired, plus buzzer and buttons on a breadboard first for Y/N.
Handwritten labels on the buttons, record stuff in the database.

**How to create a database in the arduino?**



#
##### Immeditate needs:
I am hoping to capture the attention of someone. I don't know who or why or how, but I am trying to listen creatively where I can for opportunities.
