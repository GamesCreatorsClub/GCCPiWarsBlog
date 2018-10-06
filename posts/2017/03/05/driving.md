<html><body><h1>Then</h1>
To drive our rover, we created a program that used mqtt messages to send servo signals. Pygame "game" (an application) with key controls was used to send the messages, and move the right wheels in the right places using methods that look like this:
<blockquote>
<pre>def slantWheels():
    wheelDeg("fl", 60.0)
    wheelDeg("fr", -60.0)
    wheelDeg("bl", -60.0)
    wheelDeg("br", 60.0)</pre>
</blockquote>
In the mean time we had introduced wheels service. Service that will control appropriate servos depending on which angle we want each wheel to be at and at which speed we would like to drive the wheel at.

This was okay, but it meant that we had to send the details for every individual wheel, all the time. Then came agents, which really helped with this problem. An agent is a program sent to the rover that sits and waits for a small messages from the client and communicates with other services. Since both agent and services are on the same Raspberry Pi their communication is quick even if there are many messages to be exchanged like setting up wheel positions and speeds for all the wheels all the time.

The drive agent was listening for messages like: drive &gt; forward; and sent messages for the wheels to turn to the right angle and drive at said speed. This made everything run much faster because we didn't need to constantly send many, bigger, messages from a controller (a laptop in this case) to the rover, which took up a lot of data traffic over the, somehow constrained, WiFi. Also, it would increase latency between user input and rover reacting.

As the servos all had different values for different angles, we decided to make our "wheels service" calibrate-able. We made a program that changes the calibration of the wheels. But how did we calibrate the wheels? We made a dictionary with all the wheel names as a keys, where each wheel value/data was another dictionary, with more entries. One entry was for the wheel speed:
<blockquote>"0rpm": 155,

"-300rpm": 0

"300rpm": 300</blockquote>
and another similar one for the wheel angles. Using a bit of calculations, you can work out any given angle or speed from these calibrations. This meant that all the wheels could rotate at the same speed, and turn at the correct angles.

Aside of the code, with a bit of cardboard we made our first moving, completely controllable prototype:

{{% media url="https://www.youtube.com/watch?v=_J8mJ6Y7tDs" %}}
<h1>Now</h1>
Using pyros, we promoted our 'drive' agent (the program on the rover) to the service that always runs and listens to commands such as: orbit&gt;10,200; which orbits the rover at the speed of 10, around a point that is 200mm in front of the rover.Also, we can turn left or right by adding 90 degrees to the calculated angles so rover can turn around a point to the side of it.

Wheel angles can be calculated using the same maths:

<img class="alignnone size-full wp-image-538" src="/2017/03/maths.png" alt="maths.png" width="3840" height="2160">

Both the inner and outer wheel angles need to be calculated separately. As the wheels are 73mm apart, when we calculate the distance of a point we are going to rotate around from the middle of the rover we need to subtract 36.5mm for the inner wheels and add 36.5mm to the outer wheels.

Aside of turning/orbiting we have many other commands that are much more advanced than before. Currently we have following commands:
<ul>
<ul>
	<li>drive - goes any direction (at a specified angle), facing the same way</li>
	<li>forward, back - goes forward and back</li>
	<li>rotate - turns on the spot</li>
	<li>orbit - orbits around a point, while facing it the whole time</li>
	<li>steer - turns around a point to the side of the rover</li>
	<li>stop - stops!</li>
</ul>
</ul>
Here is it in action:

{{% media url="https://www.youtube.com/watch?v=QdfC0HQn_GA" %}}
</body></html>