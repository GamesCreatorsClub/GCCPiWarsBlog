<html><body><p> 

Finally got a short break between tinkering with OpenCV, complaining about sensors and fine tuning the rover logic for <a href="https://piwars.org/2018-competition/challenges/somewhere-over-the-rainbow/">Somewhere Over the Rainbow</a> challenge to write up about our take on <a href="https://piwars.org/2018-competition/challenges/the-duck-shoot/">the Duck Shoot</a> challenge.

We started with and with very limited knowledge about Nerf guns. The only thing that was provided for the club were five packs of Nerf darts so we had some idea of the size we needed.

The idea was to have two concave cylinders with gears on bottom and a grove on the other so some kind of belt (elastic band) can be used to turn them). <img class="alignnone size-full wp-image-1201" src="/2018/02/gun-1.png" alt="gun-1" width="1280" height="817">
<!-- TEASER_END -->
The nerf dart would be somehow delivered in between two spinning cylinders and would be then propelled forward through the barrel. We started with designing a concave cylinder in Sketchup first as it seemed to be hardest challenge of all. Fortunately it wasn't half as hard as we thought.

Next was to create a gear at the bottom of the cylinder. There even was a simple gear making plugin for Sketchup available. It turned out to be slightly a longer exercise as one of the options was to use RC brushless motor (with ESC to drive it) with existing pinion. That  pinion's gears are standard gears with MOD of 0.5. This made us have to learn about engineering and gears a bit so we don't end up stripping the teeth on our 3D printed gears. Even half a millimetre discrepancy would be disastrous.

<img class=" size-full wp-image-1274 aligncenter" src="/2018/04/gears.jpg" alt="gears" width="414" height="294">

Luckily we stared with cylinders of exactly 40mm diameter, which by the formula (N = PCD / MOD) turned out to need 80 teeth exactly (40mm/MOD 0.5 = 80 teeth).

Since we have access to a 3D printer that can print two materials at the same time (<a href="http://www.cel-robox.com/">CEL Robox</a>) of which one can be flexible (ninja-flex). We went one step further and designed our cylinders to have a 'coating' of the rubbery ninja-flex material on the inside of the concave part - for better grip! Here is first prototype of it:

<img class="alignnone size-full wp-image-1277" src="/2018/04/theduckshoot-gears-1.jpg" alt="theduckshoot-gears-1" width="1280" height="847">

Next was to put all together: feathering shaft of 250 size Align clone helicopter with bearings, big slab of plastic and top housing and here is our first go at The Duck Shoot:

 

[gallery ids="1276,1278" type="rectangular"]

Power is 2800KV brushless motor which we still didn't drive a the fastest speed in fear of stripping gears heating would soften the plastic (first prototype was done in PLA). Initial results were phenomenal:

{{% media url="https://www.youtube.com/watch?v=80X3ejnvkLo" %}}

Next was to mount it on to the rover. We kinda rushed this bit...

We printed gears in ABS with and dropped grove at the top:<img class="alignnone size-full wp-image-1279" src="/2018/04/gears-robox.jpg" alt="gears-robox" width="1280" height="720">

Picked the biggest servo we have to move whole contraption up and down and quickly designed mount for it:<img class="alignnone size-full wp-image-1280" src="/2018/04/mount1.jpg" alt="mount1" width="1280" height="720">

Added made adapter for existing nerf gun magazine (only cheat) and feeder driven by a servo:

{{% media url="https://www.youtube.com/watch?v=gPi4gLOHLWU&amp;feature=youtu.be" %}}

Printed another 'top' for the rover - this time with 0.5mm thicker walls and mounted the whole lot on in:

<img class="alignnone size-full wp-image-1281" src="/2018/04/complete-top.jpg" alt="complete-top" width="1280" height="720">

The wiring of two servos and an ESC was a small problem mainly because our robot only has two 'spare' servo connections provided by the Raspberry Pi while all three would need servo signal to drive them. Fortunately, in parallel with this we were developing three sonar sensors + two servos i2c enabled breakout board. Those two extra servos slotted in perfectly in the mixture.

And as a chery on the cake came the code in our jcontroller service which controls not only elevation and trigger servos, but speed of the cylinders, too!

At least with all above we were able to say: 2 done, 5 more to go!

{{% media url="https://www.youtube.com/watch?v=bTnlL-iox7o" %}}

 </p></body></html>