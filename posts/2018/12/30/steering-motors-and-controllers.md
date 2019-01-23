Title: Putting All Together
Tags: Home, Blog, power, building, piwars
Date: 2018-12-30 10:38
Author: Daniel Sendula
Background-image: /2018/12/building-rover-1.jpg

# Putting All Together

Now it is time to continue with our build. Wheel hubs are done, design for main body is ready - it is time to put all together!

![Top Platform](/2018/12/top-platform.jpg "Top Platform"){ : style="width:100%;"}

To start with here's the first version of 'top platform' design. At the level of main body we have bulky bearings encapsulating wheels, motors that steer wheels, brushes and space for battery. Also we need place for Raspberry Pi, steering motor controllers, DC-to-DC converter, sensors, i²c multiplexer and so many other smaller things. They all will be stored on that platform.

<!-- TEASER_END -->

![Body And Brushes](/2018/12/body-and-brushes.jpg "Body And Brushes"){ : style="width:100%;"}

Now here is main body with main brushes and battery connector.

Next is to add bearings, wheel hubs, clamps to hold bearings and platform holders:

![Wheels and Platform Holders](/2018/12/building-rover-2.jpg "Wheels and Platform Holders"){ : style="width:100%;"}

Now we can add steering motors and gears:

![Steering Motors in Body](/2018/12/building-rover-3.jpg "Steering Motors in Body"){ : style="width:48%; float:left;"}
![Bottom and Gears](/2018/12/building-rover-4.jpg "Bottom and Gears"){ : style="margin-left:10px; width:48%; float:clear;"}

This is how complete body layer looks with connectors added for brushes:

![Building Rover](/2018/12/building-rover-5.jpg "Building Rover"){ : style="width:100%;"}

And when everything is covered with 'top platform':

![Building Rover](/2018/12/building-rover-6.jpg "Building Rover"){ : style="width:100%;"}

## Steering Motors and Controllers

![Stepper Motors](/2018/12/15mm-geared-stepper-motor.jpg "Stepper Motors"){ : style="width:30%; float:right;"}
Tiny geared motors used in the body for steering are as well "Plan B" motors. Original idea was to go with geared micro stepper motors but that caused a few issues: they weren't fast enough, they were strong enough and there was an issue if driven too fast - there wasn't a suitable feedback that motor didn't really move whole step as required. Too many little problems that were supposed to be fixed. Original idea was that wheels would have some 'starting point' a contact to denote particular position or tab to cut IR LED/IR Photo Diode setup similar to old fashion mice (there was a wheel with slots moving in the middle of such setup). Wheel would at the start up move to such position and after that counting steps we would be able to say exact position of the wheel. But, if stepper can mis a step - whole idea would fall through as wheel might end up being in completely wrong position to what system believes it is. So, AS5600 + simple DC motor is solution forward.

Now, we've got a few kind of micro geared DC motors for previous PiWars. Some marked as '150RPM', some '200RPM' and some '300RPM'. Others are marked as geared as 20:1 and 50:1 and both were geared 'faster' than 300RPM. All at 6V. Plan was to pick the fastest and make it turn the wheel. So, we started with 200RPM. Motors turned wheels well @ 5V (we started with simple phone charger voltage) but not fast enough. Then we switched it to 50:1 geared motor and it couldn't turn it at all. It wasn't strong enough!

Over time we moved to battery's 8V (actually 9.4V from another AC/DC adapter first) and 200RPM motors performed adequately but was that the best we could do?

Then due to some unidentified issue one geared motor got stuck and brew motor controller (H bridge)! That prompted software change - if wheel doesn't reach required position for couple of seconds, it would be switched off for another couple of seconds for H bridge to 'cool off'. At the same time tiny motor was replaced but replacement didn't behave as expected: occasionally it struggled with load! After closer inspection we saw it was 300RPM motor - and that sorted it: '200RPM' we have originally put are the fastest we can go with.

It seems that there are plenty of improvement pending in that area as well - provided we have time. 'Only' thing we need to do is to source slightly better, stronger and slightly faster motors to replace these tiny geared (to '200RPM' @ 6V) motors... Only...
