Title: We Have A Movement
Tags: Home, Blog, piwars
Date: 2019-01-14 20:57
Author: Daniel Sendula
Background-image: /2019/01/we-have-a-movement.jpg

# We Have A Movement


{{% media url="https://www.youtube.com/watch?v=NZeGPYGKcpU" %}}

<!-- TEASER_END -->

Finally some breakthrough. Rover is now fully drivable. But that wasn't straight forward journey. And the last let involved updating/fixing `drive` and `wheel` services.

The `wheel` service is responsible for steering individual wheels and driving wheels. Original code for rover M16 (with nickname 'type a' or 'type b') was driving wheels using servo signal through servoblaster daemon. This version (nicknamed 'type c') has completely different mode of delivering signals to wheels.

## Steering

Each wheel is steered using H bridges to drive micro geared DC motors with feedback through AS5600 - a digital magnetic potentiometer. So, `wheel` service now needs to drive tiny motors to position requested (through MQTT topic 'wheels/deg' or 'wheels/all` for combined input for all wheels steering and speed details). Also, it is not just simple on/off function - motors need to be driven precisely to requested position without overshooting and one of the widespread adopted solution is PID algorithm.

Previous post ([`It is alive`](http://piwars.abstracthorizon.org/posts/2019/01/05/it-is-alive/)) show rover moving around, but wheel movements weren't the most optimised. If you gave command to go at bearing of 0º and then at 180º it would stop and turn wheels for 180º and drive motors forward again. Funniest thing was that it would drive motors through shortest path and some wheels would, thus, turn clock wise and some anti-clock wise. Not the most optimal solution. Correct thing turns to be checking if required change in wheel's orientation is less or more than 90º. If less than 90º then wheels should move to that new bearing. If over 90º then smaller angle would be (180º - required angle) shorter path but wheels should start spinning in opposite way. So, for state of wheels we now have:

(ϴ, direction)

and our operation to move it to next bearing can be defined like this:

```maths
                          if |ϴ - ϴ'| ≼ 90º then (ϴ', direction) 
(ϴ, direction) ⨂ ϴ' ⟾ {
                          if |ϴ - ϴ'| > 90º then (180º - ϴ', -direction)
```

(where ϴ is existing angle, ϴ' new angle and ⨂ means "drive to new bearing of ϴ'")

With that knowledge it was easy to fix the wheel steering movements.

## Driving Wheels

Second responsibility of `wheels` service is to drive main motors to move wheels themselves. Unlike simple setting up servo signal for brushed ESCs to drive main motors, now we have two more hops to go over: `wheels` service needs to 'command' each wheel using nRF24L01 to send request to each wheel with required speed and collect response which will let us know current position and status of each wheel. See our previous post for [Hub Controller](http://piwars.abstracthorizon.org/posts/2018/12/01/hub-controller/)

BTW current implementation is somehow rudimentary - we are just sending PWM value - following implementations should implement PID algorithm for maintaining required speed adjusting PWM on the wheels themselves.

## Top Platform

Since this post has no pictures so far, let's share some of a "Top Platform" and wiring:

![Wiring Top 1](/2019/01/rover-wiring-top-1.jpg "Wiring Top 1"){ : style="width:48%; float: left;"}
![Wiring Top 2](/2019/01/rover-wiring-top-2.jpg "Wiring Top 2"){ : style="width:48%; margin-left:10px; float: right;"}

. . .

![Wiring Inside 1](/2019/01/rover-wiring-inside-1.jpg "Wiring Inside 1"){ : style="width:48%; float: left; margin-top:20px;"}
![Wiring Inside 2](/2019/01/rover-wiring-inside-2.jpg "Wiring Inside 2"){ : style="width:48%; margin-left:10px; float: right; margin-top:20px;"}

. . .

![Wiring](/2019/01/rover-top-side-view.jpg "Wiring"){ : style="width:100%; margin-top:20px;"}

## Fuse

And one more thing - it is never too much when you are cautious with LiPo batteries. That's the reason all our rovers have a fuse installed:

![Wiring](/2019/01/fuse.jpg "Wiring"){ : style="width:100%;"}

Main reason for it is custom wiring. In case there is any short of any kind, and power source for RPi is connected to battery directly, along with distribution wires for H bridges that steer wheels and last but not least slip rings and brushes that transfer power to wheel hubs. Any of those along with connections can potentially cause a short. And LiPo batteries can deliver quite a high current burning wires, but what's even worse - internally as well making them hot to the point they can explode. So, as it is "better safe than sorry" our rovers are protected with car fuses. Now, it is on us to measure total current rover normally pulls and provide appropriate fuse size. Currently we have only 5A...
