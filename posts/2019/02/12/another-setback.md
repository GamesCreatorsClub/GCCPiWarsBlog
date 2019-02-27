Title: Another Setback - The Speed of i²c Bus
Tags: Home, Blog, piwars
Date: 2019-02-12 21:08
Author: Daniel Sendula
Background-image: /2019/02/dual-i2c-multiplexer-1.jpg

# Another Setback - The Speed of i²c Bus

Current configuration of our buses (as explained [here](http://piwars.abstracthorizon.org/posts/2018/12/10/i2c-multiplexer/))
assumes that one i²c on separate PiZero is to read 4 x AS5600 magnetic sensors and 8 x VL53L1X. Reading 4 magnetic sensors 
worked well - sensors were read at approximate frequency of 250 Hz. As there are four wheels (four AS5600 magnetic sensors) for each wheel it equates to controlling it 60-ish times a second. 

But, as soon as we connected VL53L1X sensors frequency dropped to 10 times a second per wheel. It seems that VL53L1X is quite a chatty (seen through its driver) and it swamps i²c bus for setting up ranging and reading results. Even when we wait appropriate time and get to read after ranging is done!

## Solution

![Two Raspberry Pis](/2019/01/rpi3-pi0.png "Two Raspberry Pis"){ : style="width:100%;"}

Driving wheels at 10 Hz is not really an option and we have two Raspberry Pis at disposal: PiZero for motors and RPi3B+ for everything else. Since there's nothing else to be read from i²c on RPi3B+ we can just redesign i²c multiplexer board and introduce another i²c multiplexer for VL53L1X chips only. This way we'll use one PCA9545A for AS5600 connected to PiZero's i²c bus for As5600 magnetic sensors and one PCA9545A attached to RPi3B+'s i²c to read from VL53L1X.


![Multiplexer Wiring](/2019/02/dual-i2c-multiplexer-1-b.jpg "Multiplexer Wiring"){ : style="width:100%;"}

This is how it was wired this time.

![Multiplexer Bottom Side](/2019/02/dual-i2c-multiplexer-2.jpg "Multiplexer Bottom Side"){ : style="width:48%; float: left;"}
![Old Multiplexer](/2018/12/i2c-multiplexer-5.jpg "Old Multiplexer"){ : style="width:48%; float: right;"}

Main problem this time was how to fix it back instead of old multiplexer. It is a bit of tight squeeze there but at the end it managed to be pushed in there. Along with all the connectors. This is how it looks when mounted in the rover:

![Multiplexer In Rover](/2019/02/dual-i2c-multiplexer-3.jpg "Multiplexer In Rover"){ : style="width:100%;"}

PiZero now can continue controlling wheels at 60-ish Hz while distance sensors are read from RPi3B+, where, after all, they are going to be consumed by some agent. Frequency of reading VL53L1X sensors is still around 10 Hz but at least we have nice overview of what is going on all around the rover. 

![Radar](/2019/02/radar.png "Radar"){ : style="width:50%; display: block; margin-left: auto; margin-right: auto;"}
