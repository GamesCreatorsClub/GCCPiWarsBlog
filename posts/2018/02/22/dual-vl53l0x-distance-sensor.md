<html><body><span style="color:#ff0000;"><strong>Update:</strong> </span>there is important update section at the end of the article!

One of the options we are exploring for our rover to find its position is with a distance sensor. Last time we went with single <a href="http://www.st.com/en/imaging-and-photonics-solutions/vl53l0x.html">VL53L0X</a> attached to a servo. The idea was that moving it around from -90º to 90º we could scan the rover's surroundings and make decisions. And it worked.

https://www.youtube.com/watch?v=Hwwsh7aNXWM

We were able to go through the maze relatively quickly, but observing other competitors we've noticed that those that had 2 or even better 3 distance sensors were able to move much quicker as their position relative to edges and end of the corridor were relayed at the same time. So, this time we decided to have two distance sensors. The original idea was to use ultrasonic sensors (<a href="http://www.micropik.com/PDF/HCSR04.pdf">HC SR04</a>) and a breakout board with a ATmega328p connected to the RPi with i2c; More about it in one of the following blogs. In the mean time, until our i2c solution is finished, we've created plan B solution - two VL53L0X sensors on the same i2c bus. Here's the physical design of the holder <a href="https://gccpiwars.wordpress.com/2018/02/10/prototyping/#TwoDistanceSensorsHolder">Prototyping</a>.

But adding two VL53L0X on the same i2c bus is not a simple thing. Unlike many i2c devices, VL53L0X doesn't have a selectable i2c address (usually jumps on the board). It uses separate pins for a logical 'enable' (or in this case 'disable'). Pin XSHUT needs to be set to logical '1' for the sensor to use i2c bus. Internally, it has a pull-up resistor so if not connected it will still work. Setting XSHUT to logical '0' (GND in our case) makes it disabled.

Since we only need two VL53L0X sensors it can be done by simple 'not' gate - where one sensor is directly connected to the spare GPIO of the RPi and another through the 'not' gate. Simple logical circuits are easy to find and use, but in this case would take slightly more space than needed as the same can be achieved by simple transistor!

Here is the schematic for our little adapter for two sensors:<img class="alignnone size-full wp-image-1187" src="/2018/02/not-gate.png" alt="Not-Gate" width="800" height="551">FET transistor 2n7000 (which, btw, I somehow had in box of spares) seemed to be ideal for the 'not' gate.

After sketching the circuit, it was just matter of putting all of it on the breadboard and testing it out. It would have been a 10 minute job if someone didn't forget that <a href="https://github.com/richardghirst/PiBits/tree/master/ServoBlaster">Servoblaster</a> on a RPi does not release ports even if they are not actively driven. So, after half an hour of scratching heads and spare, not used, GPIO identified (GPIO 4, which, just to make things worse <strong>was</strong> set as a one wire interface!) things finally started working!

<img class="alignnone size-full wp-image-1186" src="/2018/02/not-gate-breadboard.png" alt="Not-Gate-Breadboard" width="1280" height="1299">

After proof of concept it was quite straightforward - a tiny stripboard PCB with only three lines to break would yield a 15mm x 15mm x 10mm device:

<img class="alignnone size-full wp-image-1185" src="/2018/02/not-gate-pcb.png" alt="Not-Gate-PCB" width="1280" height="1061">

Which after soldering ended up looking like this:

<img class="alignnone size-full wp-image-1184" src="/2018/02/not-gate-pcb-done.png" alt="Not-Gate-PCB-Done" width="1280" height="441">

And when is all put together like this:

<img class="alignnone size-full wp-image-1183" src="/2018/02/not-gate-pcb-done-connected.png" alt="Not-Gate-PCB-Done-Connected" width="1280" height="1129">

So here is our GCC Rover M16 with two distance sensors as originally designed here <a href="https://gccpiwars.wordpress.com/2018/01/20/2018-inaugural-meeting/#TwoDistanceSensors">2018 Inaugural meeting</a>:

 

[gallery ids="1180,1182,1181" type="rectangular"]

Now we need to <em>only</em>  write software to read both and provide values through our PyROS and a few small algorithms (like following wall to the corner or finding corner) to utilise those sensors.

<span style="color:#ff0000;"><strong>Update:</strong></span> Never read half of a blog/document. The important stuff might be near the end!

The XSHUT pin resets the sensor and enabling it again it needs some time to reset. Fortunately that time is only 1.2ms but setting up the sensor (after boot) takes some time, too, and there is time for ranging (scanning) on top of it. In the above configuration there is now a way having both sensors ranging continuously at the same time. With a not gate software sequence that would:
<ul>
	<li>reset and enable one sensor</li>
	<li>set it up</li>
	<li>read one off distance</li>
	<li>repeat for second sensor</li>
</ul>
Many would agree that this is not the most optimal way of utilising the sensor. Plus we wouldn't expect fast readings of the distances.

A much better way is to connect one sensor's XSHUT directly to VCC and the other's to GPIO and utilise another function of the sensor: the ability to change i2c address of the fly! So, after updating the board for FET to keep the VCC on the output our algorithm is now:
<ol>
	<li>make sure GPIO is high</li>
	<li>check i2c device on address VL53L0X_REG_I2C_ADDRESS + 2 is present. If so go to step 5</li>
	<li>make sure GPIO is low so only one sensor is 'online'. Other is 'shut'...</li>
	<li>change address of device on VL53L0X_REG_I2C_ADDRESS address to VL53L0X_REG_I2C_ADDRESS + 2</li>
	<li>make sure GPIO is high so both sensors are 'online'.</li>
	<li>check if i2c device on address VL53L0X_REG_I2C_ADDRESS + 1 is present. If so finish.</li>
	<li>if not change address of device on VL53L0X_REG_I2C_ADDRESS address to VL53L0X_REG_I2C_ADDRESS + 1</li>
</ol>
That way both sensors would be moved to two new i2c addresses using only one GPIO to control only one sensor. Much simpler than having extra not gate, too.

[gallery ids="1231,1230" type="rectangular"]

And output after sensors were initialised:

<img class="alignnone size-full wp-image-1229" src="/2018/02/two-sensors-working.png" alt="two-sensors-working" width="493" height="59">

 </body></html>