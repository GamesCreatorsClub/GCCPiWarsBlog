<html><body><p>Last year we had one out of three attempts on the <a href="https://piwars.org/2018-competition/challenges/straight-line-speed-test/">Straight Line Speed Test</a> challenge. Now, in a hindsight, we can blame the gyro or our lack of gyro feedback. It is so easy to spot when the mean value of every oscillating, noisy gyro is not quite on. The accumulated value tends to creep to one side. Repeated calibration usually is way to sort it (or maybe better calibration routine we never got to use).

This year we decided to do it using distance sensors. And WAY faster motors! :D

Unlike using gyroscope, distance sensors are slower but over time more accurate. At least that's a theory. Remember our distance sensor's configuration:

[gallery ids="1113" type="rectangular"]

Middle, 45º orientation is perfect for this challenge. All we needed to do was to read both distances and apply steering depending on the difference of those distances.

```python

error = distance2 - distance1
if abs(error)  steerMaxControl:
    controlSteer = steerMaxControl
elif controlSteer &lt; -steerMaxControl:
    controlSteer = -steerMaxControl

leftAngle = int(-controlSteer)
rightAngle = int(-controlSteer)
..

```

 

And results were promising. No bananas were (significantly) harmed during filming this video:

https://www.youtube.com/watch?v=R20be1AeP-E</p></body></html>