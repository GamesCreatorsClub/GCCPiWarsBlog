Title: This Year's Distraction
Tags: Home, Blog, piwars
Date: 2019-02-15 18:12
Author: Daniel Sendula
Background-image: /2019/02/dual-i2c-multiplexer-1.jpg

# This Year's Distraction

As with every year there's always something that takes our attention from what needs to be done to what is really interesting doing. 
This year we continue with last year's theme: SciFi UI! Reason more is that this time we have 320x480 5" colour touch screen on the top of the rover. And it would be waste it not being used as intended: for shiny UI and feedback!

![3.5 Touch Screen](/2019/02/35-touch-screen.jpg "3.5 Touch Screen"){ : style="width:50%; display: block; margin-left: auto; margin-right: auto;"}

## Requirements

Touch screen on rover is something new for us. So, what it can be used for?

* display some status info:
    * wheel positions
    * that controller is connected
    * that rover is running or idle
    * time on battery (uptime)
    * battery status (when we finally implement reading battery voltage and possibly current)
* display radar
* enable calibration on the rover
* enable setting up preferences
* and, maybe the most important, display some funky random images during various challenges (read PiNoon for instance)

## UI Library

First thing is to decide how to display it. There are a few GUI libraries for [pygame](https://www.pygame.org/news) for our code is all written in Python and on our Games Creators Club we use pygame for making games. But none of found libraries tick another very important box: it needs to be skinnable so we can started developing proper futuristic look and feel our rover deserves - see [last year's distraction(http://piwars.abstracthorizon.org/posts/2018/03/01/this-years-distraction/)!

Last year we did it on the client code running only, but now it needs to run on both client and rover itself...

So, since it wouldn't be proper distraction and only work on a rover itself, I decided we can make our own tiny GUI. Luckily we have a GCSE year students having Computer Science as one of the subject to help...

## Structure

GUI itself is relatively easy to make something simple for start. In its the most minimal form it consists of:

- `Component`
- `Collection`
- `Image`
- `Label`
- `Button`

and some code to put all together.

`Component` class has following methods and properties:
- `visible`
- `rect`
- `draw(surface)`
- `redefineRect(new_rect)`
- `mouseOver(mousePos)`
- `mouseLeft(mousePos)`
- `mouseDown(mousePos)`
- `mouseUp(mousePos)`

while `Collection` has extra property (and helper method):
- `components`
- `addComponent(component)`

`Image` is simple - it just has extra property called `surface` and draw methods is simple:

```python
def draw(self, surface):
    if self.surface is not None:
        pygame.blit(self.surface, (self.rect.x, self.rect.y))
```

`Label` is extension of Image where image's surface is invalidated (set to `None`) and in `draw` method re-creating image's surface to be drawn each time in a loop. When new text is set we just need to store it (again) and invalidate image.

`Button` has pointer to another component (a `Labal`) and utilises mouse methods to check if mouse is over, down and released. When mouse is finally released it will call a `on_click` callback method. Also, button can have some 'decorations':
- `background-decoration`
- `mouse-over-decoration`

These can be set to make button's background different and to respond on `mouseOver` method invocations when `mousePos` is inside of `rect` of the component. Those decorations can be set by `UIFactory` (extensions of) and if all components are created through it, that factory can, then, decorate components (in this case buttons) accordingly. It is dead easy to create special button decorations which draw funky borders, backgrounds, etc...

## Screens

So, here it is. 

From this point it was easy to extend `Component` or `Collection` and add things like:
- `Screen` - thing that can stretch over the whole screen and handle specific function,
- `CardCollection` - a collection that will show only one of components at the time (like a tab but without a control strip of buttons)

and then specific components for the rover itself that go on different 'screens':

### Main Screen

<image>

Only 'Stop' and 'Menu' buttons are displayed and only for a while. They are redisplayed when there's some mouse activity.

The main feature of the main screen is actually hidden from the view: ability to display any arbitrary image (uploaded with 'screen' service to the rover) when requested on MQTT topic. At the moment something like this would display smiley:

<iamge>

```$ mosquitto_pub -h rover6 -t screen/image -m smiley.png```

Now our challenges can have their own background images shown...

### Menu Screen

<image>

It displays buttons which in turn switch some other screens on

### Shutdown Screen

<image><image>

It displays request for confirmation (a button) and shows label saying that shutdown is in progress.

### Wheel Status

<image>

It shows positions of all wheels, odometer and wheel statuses (errors like transmitting or receiving errors, magnet strength or if wheel is stopped by software to avoid overheating of motor controller). We have special rover centric components for displaying wheels, odometers and such. Errors are displayed just as labels. For this function labels got colour as well, so error indicators can be displayed in various colours (orange or red) depending on how severe they are.

### Radar

<image>

Another special component that renders radar from 8 distance sensors.

### Calibrate Wheels

<image>

Screen that allows selection of wheel to be calibrated and then defining its orientation and position.

### Calibrated PID

<screen>

Screen that allows calibration of PID algorithm for steering wheels.

## More distractions

The moment we started doing some autonomous challenges code, rover was powered by battery and problem with LiPo batteries, if someone wants to maintain their life, is that they shouldn't be over-discharged. And, since we still don't have way of checking battery's voltage, nor measuring current through it, there are couple of other ways to fudge it in the meantime:
- measure time since battery was put in rover (read 'uptime' functionality of linux)
- measure 'idle' current through components separately (Raspberry Pis - both RPi3B+ and PiZero and rest of the system in resting state) and then measure current when motors are driven at 100% PWM. It is easy for steering as current would be similar in case of rover going around and when on the bench, but for main wheels it is bit trickier. So we decided to 'eyeball' what it might be (somewhere between stall current and freewheeling). We do it for one wheel only...

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

| rover measured mAh | time (h) | real (mAh) | type of usage |
|-------------------+|+--------+|-----------+|+-------------+|
| 1260               | 01:10    | 1100       | mostly stationary |
| 1111               | 00:37    | 780        | lots of movement  |

Some previous measurements were in similar ranges - we always calculate more than what it really is - but relatively close to what really was taken out of the battery. So far so good.

But all of it would be relatively useless if we don't have 'proper' UI to show it:

<images>

So we can throw in some other measurements like CPU load and temperature, too...

## Client UI

Same UI now can be applied to client apps. For instance, Canyons Of Mars client app can have feedback to what 'rover' really sees:

<image>

In this case there's a long corridor and nothing else. But, when corrner is detected (front left sensor detects point that is further left to what left wall is) then we can draw it, too:

<image>

Or if both front sensors detect points that are much further than wall lines (as seen from side plus back sensors) we can draw T junction:

<image>

In all of these examples back wall is drawn perpendicular to the right wall. Component is just another extension of `Component` class with specific for this case. Also, 'connected', 'Run' and 'Stop' buttons are yet another component that capsulates state of current agent - when it is running the component hides 'Run' button(s) (as there might be more buttons)

## Even More distractions

But UI itself is not enough for distraction. Rover can show nice images, we can use screen to temporarily suspend wheels operations (turning and driving), calibrate all, but... It is quite quiet stuff. What if we get tiny amplifier like [this](https://shop.pimoroni.com/products/adafruit-mono-2-5w-class-d-audio-amplifier-pam8302) + tiny speaker that can be fitted just under the top surface of the rover? That would allow our rover having sound as well.

First is to make sure RPi is sending sound to a GPIO (there are plenty of resources on the internet explaining how to do it). Second thing is to build small filter:

<image>

and wire all together:

<image>

Screen service then can gain another MQTT topic next to `screen/image`: `screen/sound`. And with following:

```$ mosquitto_pub -h rover6 -t screen/sound -m alarm_system_keypad.wav```

we can play arbitrary wav (or ogg - thanks to pygame).

But that's not enough. Right? If rovers are going to gain some intelligence (later, much later, I hope) then we need it to talk, too!

A quick search on the internet produced this: 