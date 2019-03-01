Title: This Year's Distraction - Part II - Power
Tags: Home, Blog, piwars
Date: 2019-02-22 19:57
Author: Daniel Sendula
Background-image: /2019/02/screen-status-data-collection-washed-out.png

# This Year's Distraction - Part II - Power

The moment we started doing some autonomous challenges code, rover was powered by battery and problem with LiPo batteries, if someone wants to maintain their life, is that they shouldn't be over-discharged. And, since we still don't have way of checking battery's voltage, nor measuring current through it, there are couple of other ways to fudge it in the meantime:
- measure time since battery was put in rover (read 'uptime' functionality of linux)
- measure 'idle' current through components separately (Raspberry Pis - both RPi3B+ and PiZero and rest of the system in resting state) and then measure current when motors are driven at 100% PWM. It is easy for steering as current would be similar in case of rover going around and when on the bench, but for main wheels it is bit trickier. So we decided to 'eyeball' what it might be (somewhere between stall current and freewheeling). We do it for one wheel only...

<!-- TEASER_END -->

After a session with Amp meter results are like following:

|  |  |
|--|--|
| Raspberry Pi 3B+ + PiZero | 800mA |
| Raspberry Pi 3B+ only     | 650mA |
| Rest of the rover         | 150mA |
| Steering a wheel at 100% PWM      | 300mA |
| Driving a wheel at 100% PWM and some resistance | 200mA |

Figures are rounded up and some fudged a bit.

With those information we can re-assemble some info: we know when we drive or steer each wheel and with which PWM percentage so we can start counting (measuring time) and integrate values. It is not exact current going through the motors but much better than nothing. Results seemed to be quite positive. Here are some 'rover measured' values vs real:


| rover measured (mAh) | time (h) | real (mAh) | type of usage     |
|---------------------:|:--------:|-----------:|-------------------|
| 1260                 | 01:10    | 1100       | mostly stationary |
| 1111                 | 00:37    | 780        | lots of movement  |


Some previous measurements were in similar ranges - we always calculate more than what it really is - but relatively close to what really was taken out of the battery. So far so good.

But all of it would be relatively useless if we don't have 'proper' UI to show it:

![Data Collection](/2019/02/screen-status-data-collection.png "Data Collection"){ : style="width:100%; display: block; margin-left: auto; margin-right: auto;"}

So we can throw in some other measurements like CPU load and temperature, too...

## Client UI

Same UI now can be applied to client apps. For instance, Canyons Of Mars client app can have feedback to what 'rover' really sees:

![Maze](/2019/02/maze.gif "Maze"){ : style="width:100%;"}

In this case there's a long corridor and nothing else. But, when corrner is detected (front left sensor detects point that is further left to what left wall is) then we can draw it, too:

![Maze](/2019/02/maze-2.gif "Maze"){ : style="width:100%;"}

Or if both front sensors detect points that are much further than wall lines (as seen from side plus back sensors) we can draw T junction:

![Maze](/2019/02/maze-3.gif "Maze"){ : style="width:100%;"}

In all of these examples back wall is drawn perpendicular to the right wall. Component is just another extension of `Component` class with specific for this case. Also, 'connected', 'Run' and 'Stop' buttons are yet another component that encapsulates state of current agent - when it is running the component hides 'Run' button(s) (as there might be more buttons)...
