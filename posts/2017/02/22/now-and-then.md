<html><body><p>There are so many things that happened since our inception meeting that it will take some time to catch up. Thus I decided to dedicate each blog to 'then' and 'now'.
</p><h2>Then</h2>
The rover needs wheels. The rover needs motors. The rover needs to steer. For motors we originally went with tiny 150RPM motors (@ 6V hoping to drive them at @ ~ 8V when at full speed). See below paragraph about the math that made us switch to exactly the same size motors but geared at 300RPM. For steering we went with a 9650 sized servo.

So, behold a complete GCC rover wheel:<img class="alignnone size-full wp-image-68" src="/2017/02/wheel1.jpg" alt="wheel1..jpg" width="1280" height="720">Also, wheels need tyres:

<img class="alignnone  wp-image-78" src="/2017/02/wheel3.jpg" alt="wheel3..jpg" width="312" height="217">

(Printing with a flexible material like ninjaflex is not easy job)

Here is complete wheel:<img class="alignnone size-full wp-image-77" src="/2017/02/wheel2.jpg" alt="wheel2..jpg" width="1164" height="1280">
<h2>Now</h2>
Making the rover go slowly with underpowered motors and cheap controllers is not easy job. As there is an option to swap wheels/tyres for each challenge here is one way dealing with it - making the wheels smaller! Or at least the tyres on thewheels:<img class="alignnone size-full wp-image-88" src="/2017/02/flat-tyre.jpg" alt="flat-tyre.jpg" width="1280" height="904"><img class="alignnone size-full wp-image-89" src="/2017/02/wheel-flat-tyre.jpg" alt="wheel-flat-tyre.jpg" width="1280" height="904">
<h2>Math</h2>
The PiWars challenge brings quite a chunk of maths to our club. Above change of wheel diameter let us explore how the circumference of a circle changes when diameter is changed. In this particular case wheels were 5cm and quick estimation gave us 15cm of travel distance for a full revolution, while 3cm wheel would drop it to 9cm which is almost 40% less. It is still to be seen if it will help and in which measure with challenges where we need to move rover more or less precisely forward.

Also, original 150RPM no matter how fast it seemed wasn't. Quick calculation with our 5cm wheels gave us following:

150RPM divided by seconds in a minute (60) is 2.5 revolutions per second. 2.5 times 15cm is 37.5 (give and take error - around 40cm). That would mean at least two seconds (remember - driving motors at 8V instead of 6V) per metre (0.5 m/s). For 7m it would take about 14 seconds! A lot! So, 300RPM is 5 revolutions per second, 75cm. So with some luck with higher voltage it might push to 1 m/s and drop 7 metre run to 7 seconds. Much better.

BTW - higher voltage usually means more heat and more chances for something to go wrong (motors to burn out, brushes to wear, etc). But as it is more or less only challenge where motors are going to be driven at full speed (or about) and for only 6-7 seconds it shouldn't be that bad. Rest of the challenges rover should go slower - let's say up to 50% of speed or for some much less.</body></html>