Title: Brushless Motor Controller
Tags: Home, Blog, brushless-controller, brushless, 3-pahse, piwars
Date: 2018-10-04 9:22
Author: Daniel Sendula
Background-image: /2018/10/SketchUp-Plus-Shape.png

# Brushless Motor Controller

Moving rover is the most important thing followed only by ability to steer and control. Our idea is to use gimbal brushless motors () in wheel hubs to drive rover. Ideally, as brushless motors they are quite fast, but as gimbal motors they are able to move very precisely for minute changes in camera movements.

For controller we have chosen L6234 - three phase with 5A peak current through it. Original rover had four motors that each has stall current at around 800-900mAh. With rover that might now be twice the weight it shouldn't really go over twice that, so 5 amps seems quite sufficient.

## Controller

For controller that can fit in wheel hub we have chosen ATmega328p - there are enough free pins to cover 6 pins needed for L6234, 6 for nRF24L01 (more about it later) and a few spare for ADC and such. Also, it has three timers (two 8bit and one 16bit) all with two PWM outputs. Two eight bit timers with their corresponding PWM outputs work perfectly in this situation. Remaining 16 bit timer can be, then, used for internal timing...

## Moving Motor

