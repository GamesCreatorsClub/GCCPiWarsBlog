Title: GCC Rover M18 - The Design
Tags: Home, Blog, m18, piwars, nikola
Date: 2018-10-03 20:10
Author: Daniel Sendula
Background-image: /2018/10/confidential.png

# GCC Rover M18 - The Design

![PiWars](/images/piwars-logo-small.png "PiWars"){ : style="float:left;width:25%;margin-right:30px;"}

Great news! We have been accepted for [PiWars](https://www.piwars.org) 2019 and in nothing other than the Advanced category!

It seems that our ability to make unique designs and make a rover ready and on time for two competitions
with (some) success in overall score brought us a place. There were over 150 applications
and Mike and Tim (the organisers of the PiWars competition) had to pick 30-odd competitors for the first day
(Schools and Clubs) and similar amount for the second day (Beginners, Intermediate and Advance category competitors).
So, getting there wasn't a small feat!


## The Design

This time we'll try to attempt something which nobody else did before. And it requires lots of engineering
and programming effort. Also, this time we have extra members to help us with it.

Luckily, some of our existing code (and hardware) is at our disposal, so not everything has to be made from scratch.

It is going to be new, different, challenging... It might even deserve a code name this time (hey, team, wake up!)...
But for now it is just a next rover, next generation rover or simple M18!


### Design Goals

Here are new design goals:

- 4 independently steerable wheels (4 is good number for stability)
- Wheels must freely and continuously rotate 360º (or any number given battery life) in any direction.
- Wheels should be able to 'steer' in about or, preferably, less than a second for 90º. Ideally no more a second for 180º.
- Wheels should have absolute positioning on them.
- Wheel steering should be absolutely positioned as well.
- Wheels should be powered with, preferably brushless, motors that can drive wheels so rover moves at the rate of 3.5m/s.
- Wheels should be able to move motors so rover can move with a resolution less than 1cm in each direction.
- Ideally wheel motors should be able to accelerate rover at the rate of 3.5m/s²
- 0.9g (9 m/s^2) acceleration would be nice too but maybe rather optimistic
- Centre of gravity should be as low as possible - less than 40 degrees above the contact points of the wheels
- It should have flashy lights.
- It should have sound.
- It should have a display for funny faces and serious commands and feedback.
- It shouldn't have front and back and sides should be the same.
- It should be able to track its position and orientation to precision of at least 1cm/1º in each direction at rate of 50 to 100 times a second.
- It should accept direct commands over bluetooth (joystick/controller/gamepad) and UDP.
- It should have at least rudimentary battery voltage measurement and preferably total current measurement. THat can be done by extra ATmega328p.
- It would be really nice all power to the rover to go through 'power controlled' relay so it can be switched off completely programmatically. 


### Implementation Ideas

And here is how some of them can be done:

- Four independent wheels of about 6.5m diameter (~20cm circumference)
- Sitting in four hubs rotating on four as thin as possible bearings
- Bearings that should handle both axial and radial load
- Each wheel will have power delivered to it using copper strip and brushes
- Each wheel driven by gimbal brushless motor
- Each wheel motor driven by home built brushless controller
- Each brushless controller driven by ATmega328p
- Each ATmega328p should read of contactless potentiometer (magnetic)
- Each ATmega328p should communicate wirelessly (2.4GHz) with main RPi
- Each ATmega328p should be able to drive motor slowly and very quickly (see above) and transfers back pot info to the main RPi
- Each wheel hub should be rotated with a brushed motor (steering motor)
- Each wheel hub's steering motor should be driven by one channel of a dual H bridge (4 motors - two dual H bridges)
- Each wheel hub should have magnet which is read by stationary contactless potentiometer
- As four contactless potentiometers are needed and they are communicating with i²c interface on one fixed address there's a need of 4 way i²c multiplexer
- Each side of the rover will have a distance sensor (preferably ToF)
- Rover will have 9dof sensor (accelerometer, gyro and compass)
- Rover will use any other possible means for determining precise location and orientation
- If needed more than one Raspberry Pi will be networked together use USB: Raspberry Pi 3B (or 3B+) to be used as main and
one or more Raspberry Pi Zeros in USB/Ethernet gadget mode
- Rover is to be powered by 2S or 3S LiPo battery (of at least 1000mAh capacity - preferably 2000mAh or more)


### Plan Bs

... and Cs and others...

More than one item above might not work. Here are some thoughts of Plan B scenarios:

- If a gimbal brushless motor cannot deliver required speed then it can be replaced by 'ordinary' brushless motor. Or as plan C with brushed motor that fits wheel hub's envelope.
- If copper strips and brushes do not work appropriate battery is to be sourced and placed inside of the hub (increasing its weight so speed of turning can be affected)
- If small brushed motors cannot turn hub or turn it quickly enough they can be replaced with bigger brushed motors or appropriate brushless motors driven by brushless ESCs
- If wheel brushless motor has to be replaced with brushed motor for inside of wheel hub, then homebrew brushless controller can be replaced with dual H bridge breakout board (where both 'channels' are connected together)
- If ToF sensors are slow then they can be replaced with 'fast' ultrasonic sensors or supplemented by some. Extra board (ATmega328 again) should be used in that case


### Software

Aside of existing [Pyros](https://github.com/GamesCreatorsClub/GCC-Rover/tree/master/pyros "PyROS") software, there are some aspirations we would like to achieve in the code:

- Ability to steer rover in any direction at any time
- Ability to rotate rover over any arbitrary point in space around the rover including (0, 0) - rotating over its centre
- Ability to accurately track its position given initial position and heading
- Ability to plot its surroundings given distance sensors
- Ability to 'record' positions and paths through the time
- Ability to smartly (still miss unexpected obstacles) replay recorded route

![New Rover Prototype](/2018/10/pixelised-rover.png "New Rover Prototype")

## Conclusion

As you can see - it is very, very ambitious and even if we succeed in half of the points we will have quite a unique rover. So, let's start with the making! 