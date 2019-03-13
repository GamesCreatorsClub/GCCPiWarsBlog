Title: Power Consumption And Other Woes
Tags: Home, Blog, piwars
Date: 2019-02-28 17:46
Author: Daniel Sendula
Background-image: /2019/02/temp-full-critical.png

# Power Consumption And Other Woes

Now we have [Feedback Status Screens](http://piwars.abstracthorizon.org/posts/2019/02/22/this-year-distraction-2/) sorted, something cropped out - pretty much immediately. Our Raspberry Pi is 3B+ model, 1.4GHz quad core ARM processor - something really fast and quick to generate heat. Since Raspberry Pi 3 we started needing heat sinks as Raspberry Pis can go really fast and generate lots of heat. Unfortunately our design isn't the best regarding heat dissipation as RPi's processor is sandwiched between RPi's PCB and the screen:

![Touch Screen](/2019/02/35-touch-screen.jpg "Touch Screen"){ : style="width:100%;"}

When processor is taxed a lot, heat would be trapped between these two and slowly raise. With our GUI improvements not only temperature warning came up but we finally saw CPU usage - it was over 50% which translates to more than two full cores running at around 100% (or nicely spread over all cores). With such CPU usage we started hitting temperatures of CPU around 75ºC to 80ºC and at 80ºC where CPU throttling started kicking in. 

<!-- TEASER_END -->

## High Temperature and How to Fight It

Observations provided us with fact that CPU doesn't really hits those temperatures immediately but over course of 5
10 to 15 minutes or more depending on ambient temperature. So, not everything was lost.

First idea was to try to add small fan which would fit in. Something like this: 

![Small Fan](/2019/02/small-fan.jpg "Small Fan"){ : style="width:100%;"}

But question is - where really? Inside of the rover is quite a mess and there wasn't originally left any space for the fan, even
that tiny as the one above. Here on the picture there are only two places identified: one above next to audio connector and one below:
![Inside Mess](/2019/02/inside-mess.jpg "Inside Mess"){ : style="width:100%;"}

But both have connectors taking places where fan would reside.

As ever - that required Plan-B: better usage of CPU. Culprit of high CPU usage was identified (position service - running data acquisition through two SPI interfaces at 230Hz+ as well as running Madgwick and Kalman filter algorithms). And that service was running all the time. So, now it got `pause` and `resume` MQTT messages. Also, all the wasteful PyROS MQTT processing(*) was toned down for services that do not need quick response like `shutdown`, `discovery`, `storage`, `power` and `wifi` services. They all can 'sleep' (release CPU) for longer periods of time since reacting on MQTT message on time (unlike `wheels` and `drive` service) is not critical. 

| |
| --- |
| Note: (*) paho-mqtt Python package that is normally used for MQTT communication has something implemented in not the best/more optimal way: it doesn't wait for socket data in a way that would release CPU, but continues to poll using CPU all the time. Even though [`client` object's loop() method](https://www.eclipse.org/paho/clients/python/docs/#network-loop) suggests some timeout and in many similar implementations around the programming languages and frameworks that means sleeping until activity - here it doesn't. |

### Power Sorted

With all improvements in the code we ended up with CPU usage when rover is idle at around 10% which immediately reflected on temperature - it barely went over 60-65ºC. We are expecting that position service will be needed in autonomous challenges only and for duration of the challenge - while all challenges should, really, finish without a minute in the worst case scenario.

## First Breakdown

And inevitable happened. While trying to implement following 'right wall' one of the wheels started reporting overheating protection kicking in (*). Closer inspection showed it was slower to steer than others and producing a noise that by any terms didn't sound right. It warranted nothing less but slowly taking things apart and finding out what is causing it. Worst worries were that it is one of second hand, expensive, really heavy, bearings that finally gave.

Taking wheel apart prompted another though - designing with repairability in mind. It turns out that all the wheels are assembled from the top, then middle platform added, then many other things piled on the middle platform - so getting to the wheels requires taking top platform off (which itself is not an issue), taking many wires off (which is an issue), taking PiZero off so we can take main Raspberry Pi off just go get to screws to get the top platform off. Had those screws were on each side of main Raspberry Pi it would require no dissembling of middle platform. Then, when top platform is off - there's only power supply that needs to be disconnected (and that's only positive thing). After that, we can access wheel insides from the top... But, had we had that in mind designing wheel hubs, main screws to take inner ports of the wheel out would have been at the bottom and one would be able to take the wheel stuff just by flipping rover up side down and taking wheel hub apart.

However - there was nothing wrong with the wheel hub itself. After checking how brushes push whole wheel with bearing to 'outside' it turned out that outside clamp was the one that caused all the issues - wheel hub started rubbing inside of the clamp:

![Problem](/2019/02/breakdown-3.jpg "Problem"){ : style="width:48%; float: left;"}
![Inside of clamp](/2019/02/breakdown-1.jpg "Inside of clamp"){ : style="margin-left:10px; width:48%; float:clear;"}

It turns out that original print has an error - something moved for 1mm and caused bearing to move more inside than expected and that made wheel hub to start rubbing inside of the clamp.

![Outside of clamp](/2019/02/breakdown-2.jpg "Outside of clamp"){ : style="width:100%;"}

Odd thing is that it happened only now - after hours and hours of rover going around. Maybe it finally bumped into something which caused bearing to 'fit better' where it was supposed to be... Who knows.

BTW this is what clamp (double one) is designed as:

![Outside of clamp](/2019/02/double-clamp.jpg "Outside of clamp"){ : style="width:100%;"}

### Problem Sorted

Fortunately it is just a couple of hours or printing (if that much) which solved the problem. Interesting thing is that the new print has similar error but far less announced.
