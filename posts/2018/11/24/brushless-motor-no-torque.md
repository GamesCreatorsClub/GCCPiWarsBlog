Title: Brushless Motor Torque
Tags: Home, Blog, power, slip ring, piwars
Date: 2018-11-24 19:26
Author: Daniel Sendula
Background-image: /2018/10/gimbal-brushless.jpg

# Brushless Motor Torque

Another setback. A perfect idea on paper but doesn't work in practice. Driving wheels directly from brushless motor seems to be no-go. Brushless motors are known to have really good torque, especially around 80-90% of their defined 'KV' rating. But at really low speeds power is not adequate for driving out a 1kg rover around.

At low speeds the motor is really acting as stepper motor (of a kind) and tiny windings which are more than adequate at speed defined by KV constant are not performing when just directly powered. Time for another Plan B: small geared DC brushed motors we used in previous rover. They nicely fit in side of the hub and can be driven by ready made H bridges based on TB6612FNG like this:

![TB6612FNG](/2018/11/H-bridge.jpg "TB6612FNG")

It is dual H bridge with theoretical constant current of 1.2A and peak allowed current at 3.2A. If both bridges are wired in parallel it might sustain even better current through it. And from previous years of PiWars we know that stall current of small geared DC brushed motors, driven at 2S LiPo battery (~8V) is around 800-900mA. So, it should be fine. Also, that breakout board nicely fits next to ATmega328p and nRF24L01 on one side of the wheel hub.

That, now prompted another design decision - a slight improvement of the wheel hub: if there is potential for motor to change through the course of R&D of this rover wouldn't it be better if we don't have to reprint the wheel hub every time we do so? Especially now the wheel hub is printed with capture copper rings? So, here it is - now the wheel hub can be disassembled to leave empty space and innards replaced with a newer, better version in the future. Hopefully near future! :)

![New Wheel Huh](/2018/11/new-hub-design.jpg "New Wheel Huh")

Next is to design wheel holder, motor holder, wheel guards, place for controller, etc...
