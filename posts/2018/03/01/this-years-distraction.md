<html><body><p>Rovers work no matter what the 'client' side apps look like, right?
</p><h2>PyROS Clients and Agents</h2>
<a href="https://github.com/GamesCreatorsClub/GCC-Rover/tree/master/pyros">PyROS</a> (Python Rover Operating System) is, in essence, simple Linux service that starts one Python program which listens to particular topics on MQTT (local queue broker). The client (computer or laptop) program communicates with Rover's code by the same MQTT that sends instructions to that Python program (imaginatively called just PyROS). The most important command clients can send to the PyROS is to upload a whole Python code (file with file extension '.py' - a Python program). There is set of command line tools (pyros) that can do various things to the rover - upload program/service/agent, query what is running on the rover, start/stop program/service/agent, check stdout (read logs), etc..
<!-- TEASER_END -->
PyROS 'recognises' three types of programs: services, programs and agents. Services are maintained by PyROS and the service programs are started by PyROS at start up and kept running all the time. The most important services on our rovers are:
<ul>
	<li>wheels - driving servos and motors individual wheels</li>
	<li>drive - accepts 'high level' commands like drive (forward), rotate, steer and 'translates' them to the 'low level' commands - to wheels</li>
	<li>jcontroller - reads (bluetooth) joystick inputs and translates them to 'high level' commands for drive service</li>
	<li>mpu9250 (and similar) - reads mpu9250 board for gyro/accelerometer/compass and provides readouts for other programs/agents need gyro/accel input</li>
	<li>vl53l0x - distance servo  - provides read distances</li>
	<li>lights - service that turns rover lights (underneath the rover - originally intended to be used for follow the line)</li>
	<li>shutdown - service that reads a switch and shuts down the Raspberry Pi</li>
	<li>discovery - service that listens to UDP boradcasts and replies with rover IP/port and name (simple discovery service)</li>
	<li>camera - service that reads camera and sends stills to agent/program or client</li>
	<li>storage - service that when written to stores data in a tree (similar to Windows Registry) and when requested, emits values and changes to values back to all listeners</li>
</ul>
and a few others.

Unlike services, programs are a 'one off' code that is started and when stopped left alone. They are not started at start up but otherwise do not differ from services. There is one use for the programs - libraries. All the Python code on PyROS on the Raspberry Pi (including all programs/services and agents) is exposed as Python modules to each other. So, if needed (still to be considered if its good) one service can import another service directly and use their code). That means if something is uploaded as a program and it just does minimal initialisation (if needed at all!) and stops - it can be treated as a library (module) for other programs. Currently we have only two:

pyroslib - set of helper methods to subscribe/publish to MQTT queue and similar frequently used

storagelib - set of helper methods to read/write to the common storage

And slowly we closed to the last part of this tangent before getting to the distraction: the agents
<h2>Agents</h2>
The agents are closer to the programs than to the services. But, unlike programs which are 'left alone' by the PyROS agents are closely 'watched' by it. Actually not that agents are closely watched but the 'interest' in the agents is. But, let's go on another smaller tangent: what are <a href="https://en.wikipedia.org/wiki/Software_agent">software agents</a> really?

PiWars rules dictate that for autonomous challenges Robots must perform a function autonomously. That means without a help of operator or another computer. But, with PyROS we have chance to write code on a laptop and upload it for execution on the rovers themselves. Uploading a program to execute remotely is fine - but those programs are not expected to work and work perfectly immediately.  And what is the output from such, remote, programs? Normally one would use scp to copy python code, ssh to the Raspberry Pi (Raspbian Linux) and start a Python program watching the 'stdout' for the 'debug info'. With PyROS we can do the same: upload program and use the log function to read the debug information from the stdout of the remote program. But that is not as convenient, nor visually effective as it would be to run program on the 'client' (a laptop) which will on start up upload an agent to the Raspberry Pi and keep close communication with the agent using the same MQTT mechanism. Then, the client could send various 'commands' like 'start' and 'stop' for the challenge, and many other smaller, less important stuff needed for initial programming of the code for the autonomous challenge (like breaking down steps in smaller, more manageable bits) and turning on/off different debug info and presenting it in convenient form on the screen.

Back to PyROS - what PyROS does for the agents is following: after an agent is uploaded and started it will expect the client to send 'ping' message for the agent in regular intervals no longer than 5 seconds (subject to change). If a ping is missed the agent is going to be killed. That will allow client being stopped on the laptop and PyROS will take care, eventually, of the agent code.

We had a few moments when we were scratching our heads when rover was doing odd things just to realise that one of the agents kept working because we didn't have this 'automatic kill switch' in place.

And now this years:
<h2>Distraction!</h2>
Where ever we turn we can see 'quality' images of futuristic UI - screens of made up applications that run in space ships or projected space suite visors. Many SciFi films or animated films have them. For instance:

<img class="alignnone size-full wp-image-1245" src="/2018/02/blame.jpg" alt="blame" width="1280" height="800"> Still frame from the anime <a href="https://en.wikipedia.org/wiki/Blame!_(film)">Blame!</a>

or

<img class="alignnone size-full wp-image-1246" src="/2018/02/expanse.jpg" alt="Expanse" width="1280" height="609"> Still frame from the TV series <a href="http://www.imdb.com/title/tt3230854/">The Expanse</a>

We have a small collection of applications that are used for our rovers. Almost every autonomous challenge has client application (sometimes called 'the controller'). Like one we started writing for the <a href="http://piwars.org/2018-competition/challenges/somewhere-over-the-rainbow/">Somewhere Over the Rainbow</a> challenge:

<img class="alignnone size-full wp-image-1254" src="/2018/02/camera-example-old1.png" alt="camera-example-old" width="1024" height="819">

Or application for calibration of servos and ESCs for the wheels:

<img class="alignnone  wp-image-1252" src="/2018/02/calibration-old.png" alt="calibration-old" width="390" height="531">

Or one of the latest addition - tiny application that reads local computer's joystick (or keyboard) and sends data to the drive service.

<img class="alignnone  wp-image-1253" src="/2018/02/jcontroller-old.jpg" alt="jcontroller-old" width="428" height="196">

But, even though they are functional they didn't have the style or anything that cutting edge science feel in them. And, after all, we are dealing with some kind of progress. So, there came in our little distraction. So, after a weekend of technically useless and visually pleasing work we've spruced up our UI side of our apps. So, Something Over the Rainbow controller ended up looking like this:

<img class="alignnone size-full wp-image-1250" src="/2018/02/camera-example.png" alt="camera-example" width="2066" height="1658">

Calibration app:

<img class="alignnone  wp-image-1249" src="/2018/02/selecting-rover.gif" alt="selecting-rover" width="465" height="561">

Joystick controller:

<img class="alignnone  wp-image-1248" src="/2018/02/jcontroller.gif" alt="jcontroller" width="483" height="220">

Since almost all of our apps follow very similar pattern it was very easy converting other old apps to use new UI style. Here is, for instance, app we created to test accelerometer:

<img class="alignnone size-full wp-image-1256" src="/2018/02/accelerometer.png" alt="accelerometer" width="800" height="816">

And there are a few others as well... If competition is to be won by fancy graphics, I think we would be among the winners!</body></html>