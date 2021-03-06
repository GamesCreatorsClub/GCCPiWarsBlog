Title: This Year's Distraction
Tags: Home, Blog, piwars
Date: 2019-02-15 18:12
Author: Daniel Sendula
Background-image: /2019/02/screen-wheels-status-wide.png

# This Year's Distraction

As with every year there's always something that takes our attention from what needs to be done to what is really interesting doing. 
This year we continue with last year's theme: SciFi UI! Reason more is that this time we have 320x480 5" colour touch screen on the top of the rover. And it would be waste it not being used as intended: for shiny UI and feedback!

![3.5 Touch Screen](/2019/02/35-touch-screen.jpg "3.5 Touch Screen"){ : style="width:50%; display: block; margin-left: auto; margin-right: auto;"}

<!-- TEASER_END -->

## Requirements

A touch screen on our rover is something new for us. So, what can it be used for?

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

![Main](/2019/02/gcc-rover-ui.gif "Main"){ : style="width:35%; display: block; margin-left: auto; margin-right: auto;"}

Only 'Stop' and 'Menu' buttons are displayed and only for a while. They are redisplayed when there's some mouse activity.

The main feature of the main screen is actually hidden from the view: ability to display any arbitrary image (uploaded with 'screen' service to the rover) when requested on MQTT topic. At the moment something like this would display smiley:

![Main Smiley](/2019/02/screen-main-smiley.png "Main Smiley"){ : style="width:35%; display: block; margin-left: auto; margin-right: auto;"}

```$ mosquitto_pub -h rover6 -t screen/image -m smiley.png```

Now our challenges can have their own background images shown...

### Menu Screen

![Main Menu](/2019/02/gcc-rover-ui.gif "Main Menu"){ : style="width:35%; display: block; margin-left: auto; margin-right: auto;"}

It displays buttons which in turn switch some other screens on

### Shutdown Screen

![Shutdown](/2019/02/screen-shutdown.png "Shutdown"){ : style="width:35%; display: block; margin-left: auto; margin-right: auto;"}

It displays request for confirmation (a button) and shows label saying that shutdown is in progress.

### Wheel Status

![Wheels Status](/2019/02/screen-wheels-status.png "Wheels Status"){ : style="width:35%; display: block; margin-left: auto; margin-right: auto;"}

It shows positions of all wheels, odometer and wheel statuses (errors like transmitting or receiving errors, magnet strength or if wheel is stopped by software to avoid overheating of motor controller). We have special rover centric components for displaying wheels, odometers and such. Errors are displayed just as labels. For this function labels got colour as well, so error indicators can be displayed in various colours (orange or red) depending on how severe they are.

### Radar

![Radar](/2019/02/screen-radar.png "Radar"){ : style="width:35%; display: block; margin-left: auto; margin-right: auto;"}

Another special component that renders the radar from 8 distance sensors.

### Calibrate Wheels

![Calibrate Wheel](/2019/02/screen-calibrate-wheel-combined.png "Calibrate Wheel"){ : style="width:70%; display: block; margin-left: auto; margin-right: auto;"}

Screen that allows selection of wheel to be calibrated and then defining its orientation and position.

### Calibrated PID

![Calibrate PID](/2019/02/screen-calibrate-pid.png "Calibrate PID"){ : style="width:35%; display: block; margin-left: auto; margin-right: auto;"}

Screen that allows calibration of PID algorithm for steering wheels.

| |
| --- |
| Note: (*) All pictures are screenshots of an application that runs on the client (a laptop), but exactly the same is rendered on the 320x480 screen of the rover (minus border and top bar with rover selection and GCC logo) |
