[Back](activity.md)

# Thoughts and Notes:

## Other Projects:
_(Jot down ideas for other projects here)_
- wind turbine ( Is the most popular product TinyML or IoT? )
- Continue recording _golf-swing-sensors_ progress in THIS repository
- **LED glasses** which display "HIGHLND" across them (**ORDERED!!**)
  - **Acquire** one of the mounts/PCB boards, and maybe later design PCB myself. Requires Adafruit _feather_ board. I imagine I'd document a project just like this _golf-sensors_ one, step by step. Fun to imagine Patreon or YouTube connections also. I like the idea of offering this to fans and artists. Should be very fun to do, with profit potential.
  - **INCORPORATING _KWS_** for verbal commands! Like, "highlnd" would display "HIGHLND" across the frame, and could do "party", "love", "hearts" (heart emojis), and with special design specs I could do ANY voice commands. Of course limited to device capacity. Maybe not for the _feather_ but another that includes a microphone and capacity for TinyML.
  - **Golf Bag Robot** Attach servos and a wheel to my golf bag, and make it use Image Recognition to follow me around on the golf course. Hold the flag (or something) up, and it moves forward, to the side and it goest that way, hold it down and it stops. _GOOD PROJECT!_


## Fourier
- I would like to convert MOTION recorded here and stored in JSON into a _Fourier Series_ because it seems that that would be a good way to create 3D time-series illustrations
- ([3B1B Fourier Series video here](https://youtu.be/r6sGWTCMz2k?t=1226)) The video describes 2 dimensions, but what I'm interested in calculating is in 3 dimensions.
- This formula has hyperparameters which I believe can be learned by a Neural Net. A golf swing consists of a time series of 3D points (say, 100 3-dimensional points per second), and can entirely be drawn with a **Fourier Series!**
- Technically, although virtually impossible to visualize as a human being, this will work in _**n-dimensions!**_ 

### This means?
- that a _multi-dimensional_ line can be generated, against which comparisons can be made for a VERY QUICK INFERENCE!
- To illustrate, imagine a drone on a 3D path, and add 2 more dimensions (parameters) such as temperature and pressure. Now it's a 5-dimensional line against which a model can make a quick inference!
##
The above might describe some complete science fiction. I  may be thinking about this in the wrong way. After a very superficial review of Fourier Transform, I think that "3D" aspects are asking too much. I really don't think this is what I am looking for.

### Or it's possible that...
- all I really need is something that we learned about when we were analyzing spectrograms from AUDIO samples, and transforming them using **FFT (Fast Fourier Transform)** so maybe this can be applied to my problem, where there is a singular path (2D), but with one additional corresponding parameter.
- It's actually much like combining multiple sensors, isn't it?!! After all, an IMU with 9DOF actually has 9 readings, so of course being a mere human being, I couldn't see that we would just utilze what ML already knows how to do.

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
#
#
#
#
#
**Note for repository description.** This repository is the narrative of my own progress in development, from start to... to wherever I am now. This can be a beginners guide because it's all new to me, and I am including all the stuff that I need to always remember.
#
IDEA: (I didn't know where to put this)
Use the microphone on the BLE device to turn on and off the flashlight on my phone.
Do this right after I finish attaching the flashlight to the y-axis tilt.
#
Data collection. How can Edgeimpulse be used for data collection? Prof said I could use it somehow but there's no indication about how they could collect data. They like to acquire it though. I send it up, and then they do something with it. But that doesn't make easier the data collection.
#
##### Immeditate needs:
I am hoping to capture the attention of someone. I don't know who or why or how, but I am trying to listen creatively where I can for opportunities.
