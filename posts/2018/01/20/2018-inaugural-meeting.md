<html><body><p>This time we are starting very late due to many independent factors, but our resolve never faltered. Fortunately we finished quite strong last year - with two fully working rovers. However one lagged because it was built first and not upgraded completely. We only had very little damage, mainly on some attachments (broken servos mostly) and some of these we aren't going to need this year.

The only area, though, was that we didn't do as much as possible with software! Who would tell - given we're really a software club. Eh.

Anyway, on our first meeting we went through all challenges and seeing what we had, and what we'll need to do to successfully complete them:
</p><ul>
	<li>Straight-Line Speed Test - we need new a distance sensor configuration and new software</li>
	<li>The Minimal Maze - even though we can go with our existing software, improvement in distance sensors and some new software could make it far faster</li>
	<li>Somewhere Over the Rainbow - hardware wise we have all we need (even though new distance sensors again could improve overall performance), but the software will be a challenge</li>
	<li>Slightly Deranged Golf - we need some new ways of capturing the golf ball</li>
	<li>Pi Noon - we have all we need but can improve on the way the pin is held and we could add some more 'secret weapons and moves'; we also may need much more training</li>
	<li>The Obstacle Course - we need different way of communication between the transmitter and the rover</li>
	<li>The Duck Shoot - that's the hardest challenge given that we have little time to devise shooting mechanism</li>
</ul>
<h3>Straight-Line Speed Test and The Minimal Maze</h3>
Previously we had one sensor attached to a servos which allowed 180º of freedom but for quick maze run two seem as a minimum, while the Straight-Line test would benefit of two as well as Somewhere Over the Rainbow. So, next is to design a way of attaching two distance sensors (to start with, a simple HC-SR04) and make them read accurately. Our first attempt last year was to use a Raspberry Pi to read the distance, and hardware (voltage divider) was built in PCB of the rovers. It wasn't accurate enough as the Raspberry Pi was doing so many things at the same time and the results were all over the place. We tried to replace them with specialised a infra-red i2c sensor, but its internals were poorly documented and the sensor itself wasn't cheap.

<img class="  wp-image-1114 aligncenter" src="/2018/01/sensors1.png" alt="sensors1" width="203" height="189">

Now we are planning on going with an Arduino (ATmega328p) that will act as a i2c slave and allow control of two HC-SR04 sensors at the same time along with driving one servo that will move sensors around. That will allow these three configurations:

<img class="  wp-image-1113 aligncenter" src="/2018/01/sensors2.png" alt="sensors2" width="524" height="272">
<h3>Somewhere Over the Rainbow</h3>
For Somewhere Over the Rainbow we need to finally get our secret weapon out: the camera!<img class="alignnone size-full wp-image-1112" src="/2018/01/screen-shot-2018-01-16-at-20-44-37.png" alt="Screen Shot 2018-01-16 at 20.44.37.png" width="1140" height="784">It was supposed to be used for follow the line (never worked as planned). Our first go will be in adopting existing software which captures images from the camera and scales them to 80x64 pixels and moves them to numpy for processing. The idea is to use as simple code as possible for detecting the presence and position of red, blue, yellow and green on the picture. Also, the code should be as modular as possible so we can, given enough time,  later switch to use more advanced software like OpenCV.
<h3>Slightly Deranged Golf</h3>
Last time we were lucky - in the second go we realised that kicking the ball is not as good as holding it, so this time we'll specifically design contraption to capture the ball and then kick it in the last stage of the challenge. There'll be lots of possibilities and changes for nice engineering!
<h3>Pi Noon</h3>
Last time we lost in finals! And we lost it to a bigger, better, faster rover. This time we decided to concentrate on making our rover bigger for the challenge (so nobody can pop our balloons from behind), faster (if possible given the restrictions of our hardware platform) and invest time in driving skills! So, we are already looking forward to local battles!
<h3>The Obstacle Course</h3>
Away with gyroscopes and back in with the distance sensors! Not completely but combining two is going to be the course of the day. This time we are hoping to use the gyro, but not letting the rover hit the edges by using our distance sensors.
<h3>The Duck Shoot</h3>
By priorities it ended up being last even though from an engineering perspective it is the most interesting. At the moment we are at white board planning stages and everything goes - from using toy catapult, existing nerf guns from our toy boxes to designing a bespoke solution with rapid fire of 20 nerf darts a second! Imagination on that is really limitless, but reality is that we have only 12 weeks to complete it.

 </body></html>