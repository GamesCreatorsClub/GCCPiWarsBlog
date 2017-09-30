<html><body><h2>Then</h2>
When all servos and motor controllers (that are to be commended as servos) were wired, we needed a way to control them in the software. Luckily, Raspberry Pi has brilliant piece of software called '<a href="https://github.com/richardghirst/PiBits/tree/master/ServoBlaster">servoblaster</a>' (<a href="https://github.com/richardghirst/PiBits/tree/master/ServoBlaster">open source project on the github</a>) which utilizes internal DMA timers for generating precise PWM they way the servos need. Also, it pretends to be a device by creating a pipe file called /dev/servoblaster which acts as simple interface with the 'outside' world. All that is needed,  is for someone to a send line of text in format "&lt;servo_number&gt;=&lt;position&gt;" and servoblaster will move a particular servo to the position. The servos are numbered 0, 1, 2... and in /etc/init.d/servoblaster file we can define OPTS environment variable with "--p1pins=&lt;comma delimited list of GPIO pins&gt;" to link between ordinary number and GPIO pin. GPIO pin numbers correspond to physical pins which makes it even simpler. The servo position is the width of the signal in tens of microseconds, so, 150 corresponds to 1.5ms (1500 micro seconds) which is mid point of servos.

We decided to numerate the servos like this:
<ul>
	<li>servo 0 -&gt; front right motor controller</li>
	<li>servo 1-&gt; front right steering servo</li>
	<li>servo 2 -&gt; front left motor controller</li>
	<li>servo 3-&gt; front left steering servo</li>
	<li>servo 4 -&gt; back right motor controller</li>
	<li>servo 5-&gt;back right steering servo</li>
	<li>servo 6 -&gt;back left motor controller</li>
	<li>servo 7-&gt;back left steering servo</li>
</ul>
Simple shell script like this moved out rover forward:
<pre>#!/bin/bash
# straighten wheels
echo &gt;/dev/servoblaster "1=150"
echo &gt;/dev/servoblaster "3=150"
echo &gt;/dev/servoblaster "5=150"
echo &gt;/dev/servoblaster "7=150"
# drive forward
echo &gt;/dev/servoblaster "0=180"
echo &gt;/dev/servoblaster "2=120"
echo &gt;/dev/servoblaster "4=180"
echo &gt;/dev/servoblaster "6=120"

sleep 2

# stop
echo &gt;/dev/servoblaster "0=155"
echo &gt;/dev/servoblaster "2=155"
echo &gt;/dev/servoblaster "4=155"
echo &gt;/dev/servoblaster "6=155"</pre>
With that we'd completed the most important part of this project: our rover came to life!

Next, is to code this in Python!
<h2>Now</h2>
Over the weekend (and partly over the whole half term) we did two important steps: released Android application to drive the rover and made an adapter for a wire coathanger for our local PiNoon. Next Wednesday (tomorrow) we are going to have a party! 60 balloons are ready and rules set: whoever loses the round is responsible for blowing next balloon up!

The android application is written in <a href="https://libgdx.badlogicgames.com/">LibGDX</a> in Java, so, as such, it can happily run on any PC (Windows, Linux or Mac) and be deployed as an android application or even as an iOS application! Application itself utilizes two point touch - one for the left and another for the right joystick.

<img class="alignnone size-full wp-image-240" src="/2017/02/gcc-rover-conroller.png" alt="gcc-rover-conroller.png" width="600" height="392">

The left joystick controls the rover's rotation and right forward/side movements. There are three modes:
<ul>
	<li>only right joystick is used -&gt; the rover keeps orientation,</li>
	<li>forward/back is used on right joystick and left/right on left joystick -&gt; the rover goes forward/back and turns more or less depending on left joystick</li>
	<li>only left joystick is used -&gt; the rover rotates on the spot</li>
</ul>
But we went one step further. As one cannot use two mouse pointers on a PC (laptop), we've added support for the joystick as well! Simple USB game controller, like this:

<img class="alignnone size-full wp-image-246" src="/2017/02/usb-wired-joystick.png" alt="usb-wired-joystick.png" width="500" height="333">

Now, having app and joystick and several Android devices (phones and tables) we can drive all our three rovers at the same time! Youtube videos are to follow...</body></html>