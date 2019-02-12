Title: VL53L1X Time of Flight Sensor
Tags: Home, Blog, piwars
Date: 2019-02-10 15:20
Author: Richard Gemmel
Background-image: /2019/01/we-have-a-movement.jpg

# VL53L1X Time of Flight Sensor
## A Low Cost Laser Range Finder

## Introduction
"Time of Flight" sensors are laser range finders. There are several that are available on breakout boards. We're using the VL53L1X made by [ST Microelectronics](https://www.st.com/en/imaging-and-photonics-solutions/vl53l1x.html) for detecting walls. The breakout board we're using is provided by [Pololu](https://www.pololu.com/product/3415).

![VL53L1X from Pololu](https://a.pololu-files.com/picture/0J8679.1200.jpg?01e3896017fbf0b3c22e42e0964e0570)

The [Pololu](https://www.pololu.com/product/3415) product page provides a good overview of the sensor. It also provides links to datasheets. This blog post contains the notes I made whilst testing the sensor. I won't repeat information you can find in the datasheet.

This information is not definitive. It's just my impressions from testing a sensor for a few days.

## Effective Ranges
| Distance Mode | Min (mm) | Max (mm)  | Max Sunlight (mm) |
| ------------- | -------: | --------: | ----------------: |
| Short         |       40 |      1300 |               200 |
| Medium        |       40 |      2300 |               130 |
| Long          |      250 |      2500 |                70 |

The minimum and maximum ranges are the points at which the sensor started reporting errors. In short and medium distance modes the sensor gives incorrect distances below 40 mm but doesn't report any errors. 

"Max sunlight" gives the maximum value when the target was lit by direct sunlight through a glass window. The sensor is all but useless if the target or the sensor are in direct sunlight. 

The timing budget doesn't seem to affect these distances. The material target is made of has a small effect. White surfaces can probably be detected at a longer range then dark surfaces but the effect is not very significant.

The maximum effective range falls considerably if you reduce the field of view significantly.

| Ambient Light | FOV   | Distance Mode | Max (mm) |
| ------------- | :---: | :-----------: | -------: |
| Office        | 16x16 | Short         |     1300 |
| Office        | 8x8   | Short         |     1300 |
| Office        | 6x6   | Short         |     1000 |
| Office        | 5x5   | Short         |      700 |
| Office        | 4x4   | Short         |      400 |

## Configuration
There are 3 key configuration parameters that you must set.
* distance mode
* timing budget
* intermeasurement period

Distance mode determines the maximum range of the sensor. Use Short range if possible. It's faster and more accurate. Medium range is a good all rounder. Use Long range only if you have to. The data sheet says that Short is still effective in bright sunlight but my experience suggests that it isn't.

The timing budget determines how long the sensor records and analyses data. Short timing budgets underestimate the range slightly and the readings are more variable. (The standard deviation is larger.)

The intermeasurement period defines a pause between each sensor reading. Large intermeasurement periods such as 2 seconds are great if you want to save power. In our application we wanted to get readings as fast as possible. In this case there's an optimal period for each timing budget value that maximises the read rate.

| Distance Mode | Timing Budget (ms) | Intermeasurement Period (ms) | Error (%) | Std Dev (mm) | Freq (Hz)| 
| ------------- | :----------------: | :--------------------------: | :-------: | :----------: | :------: |
| Short         |       6.5          |      10                      | -2.5      | 5.3          | 103      |
| Short         |       8.5          |      12                      | -1.3      | 2.6          |  86      |
| Short         |      12.0          |      16                      | -0.8      | 2.2          |  65      |
| Short         |      30.0          |      35                      | -0.3      | 1.2          |  30      |
| Medium        |       7.5          |      11                      | -6.3      | 4.2          |  92      |
| Medium        |      10.5          |      14                      | -3.4      | 3.5          |  74      |
| Medium        |      16.5          |      20                      | -2.1      | 2.0          |  52      |
| Medium        |      30.0          |      34                      | -1.1      | 1.4          |  30      |
| Long          |      16.5          |      21                      | -4.7      | 3.9          |  49      |
| Long          |      18.5          |      23                      | -4.7      | 3.9          |  45      |
| Long          |      33.0          |      38                      | -2.3      | 2.0          |  28      |

## Error Codes
You should reject measurements unless the measurement status code is 0 - Range Valid.

The sensor will often give reasonable looking ranges even when it's reporting errors. At other times the reported distance can be wildly erratic, swinging between min and max ranges and everything in between. As far as I can tell, the driver does a good job of telling you whether a measurement is usable. My advice is that if the driver rejects the range then you should too!

| Code | Description | Meaning           | Comments |
| ---- | ----------- | ----------------- | -------- |
| 0    | Range Valid | Successful range  |   All Ok |
| 1    | Sigma Fail  | The reading is too inaccurate to use. The driver measures the standard deviation (sigma) of the results it's getting as it builds a response. It raises this error if sigma is too high. | Rare under artificial lights. Usually happens in direct sunlight. |
| 2    | Signal Fail | The driver reports this if the return signal from the target is too weak to be used. | Happens if the distance is too great for the chosen mode. Also common in bright sunlight. I suspect it also happens if there are too many return paths. |
| 3   | Min Range Fail | ? | Never seen it. |
| 4   | Phase Fail | ? | This usually happens when the distance a bit too far for distance mode. |
| 5   | Hardware Fail | The sensor is not working. | Never seen it. |
| 7   | No Update | ? | You'll see this if you set the timing budget too short. It also occurs occasionally in normal usage. |

## Calibration
All measurements were done on a single sensor using the factory calibration.

## Driver
We're using the [GCC-VL53L1X](https://github.com/GamesCreatorsClub/GCC-Rover/tree/master/rover/vl53l1x) driver for python. This wraps the C library published by ST Microelectronics which is available from their website. The STM driver version is 2.3.1.
