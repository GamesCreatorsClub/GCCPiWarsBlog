<html><body><p>We were quite busy last few weeks. Our first priority was to make sure all rovers are operational. Motor swaps, new wheels, and other minor repairs where needed since last PiWars. And now - all three are updated to the latest spec and even upgraded. Last rover got MPU9250 9 axis gyro/accelerometer/compass breakout board and all got extra pin provided at our 'i2c' bus. Now not only we have GND, VCC (3.3V), SDA and SCL on the cable for attachments, but extra GPIO - GPIO 4 which, in theory, can dub as One Wire Interface, too. Currently that pins purpose is to select between two VL53L0X sensors.

<img class="alignnone size-full wp-image-1196" src="/2018/02/threerovers1.png" alt="ThreeRovers1" width="1280" height="542">
<!-- TEASER_END -->
Aside of making rovers up and running, we, as you have seen in previous blogs, have undergone another major change - switched from WiFi/TCP/MTQQ communication with the rover to Bluetooth directly to the rover. Now two rovers can be controlled remotely (previously we would be using a computer with wired game controller) - one with old style MQTT communication and another with PS3 game controller. That allowed us to start practising where we lost in final of one of the challenges: PiNoon.

<img class="alignnone size-full wp-image-1192" src="/2018/02/pinoon-3.png" alt="PiNoon-3" width="1280" height="970">

We undertook that task quite seriously:

{{% media url="https://www.youtube.com/watch?v=XHUsOcGC4vE" %}}

{{% media url="https://www.youtube.com/watch?v=vT-AMMzponE" %}}

{{% media url="https://www.youtube.com/watch?v=VbNclTDoOS0" %}}

Aside of having fun bursting each other balloons we had quite a serious task designing new (and secret) controls and special moves:

<img class="alignnone size-full wp-image-1190" src="/2018/02/clubactivity2.png" alt="ClubActivity2" width="1280" height="960">

Also we did quite a lot on 'behind the scenes' software regarding controllers. See here https://gccpiwars.wordpress.com/2018/02/10/our-controllers-and-why-indentation-is-important/.

<img class="alignnone size-full wp-image-1191" src="/2018/02/clubactivity1.png" alt="ClubActivity1" width="1280" height="960">

In parallel to it, we are exploring ability to use ultrasonic sensors <a href="http://www.micropik.com/PDF/HCSR04.pdf">HC SR04</a><a href="http://www.micropik.com/PDF/HCSR04.pdf">HC SR04</a> as they are much faster to measure distances than <a href="http://www.st.com/en/imaging-and-photonics-solutions/vl53l0x.html">VL53L0X</a> and theoretically equally precise. Our original idea was that Raspberry Pi would be sufficient to trigger the sensor and measure time of response, but with multi-tasking non real-time operating system it turned out to be quite messy operation.

<img class="alignnone size-full wp-image-1189" src="/2018/02/clubactivity3.png" alt="ClubActivity3" width="1280" height="960">

Because of that we started working on Arduino/ATmega328p solution where it should act as slave i2c device which will use 16-bit timer (Timer1) to measure time from trigger signal to echo. Current status update is that using Arduino Wire library for i2c and sensor library for reading ultrasonic sensor doesn't work quite well due to interrupt clashing, but some of the following blog posts are going to explore it in depth and, hopefully, announce solution.

<img class="alignnone size-full wp-image-1201" src="/2018/02/gun-1.png" alt="gun-1" width="1280" height="817">

Aside of that, we have started working on nerf gun (ground up solution) and recognising coloured balls for the <a href="http://piwars.org/2018-competition/challenges/somewhere-over-the-rainbow/">Somewhere Over The Rainbow</a> challenge.

<img class="alignnone size-full wp-image-1197" src="/2018/02/redgreenblueyellow.png" alt="RedGreenBlueYellow" width="1280" height="960">

More updates next time...</p></body></html>