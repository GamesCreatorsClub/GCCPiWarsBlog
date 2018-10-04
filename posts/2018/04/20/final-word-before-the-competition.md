<html><body><p>Preparation for the <a href="https://piwars.org/">PiWars</a> was really fun. Previous competition's anxiety - will we manage to do anything or not didn't affected us this time. It was more - can we prepare to do <em><strong>ALL</strong></em> challenged to the best of our abilities or not.
</p><h2>Rovers</h2>
<img class="alignnone size-full wp-image-1196" src="/2018/02/threerovers1.png" alt="ThreeRovers1" width="1280" height="542">

Rovers themselves turned to be really great source of fun and idea of having three for the club worked quite well. That magic number allowed us to have always one spare, one with better motors, one with the latest software (before it was replicated to the others), one with bigger wheels, one we like the best, one that always got one wire to the motor unsoldered somehow, one that had connector wires somehow shorter and harder to use than others, one with broken motor, etc...

At one point only one SD card was really working and then cloned three times. After all, for SF fans: <a href="https://en.wikipedia.org/wiki/Rendezvous_with_Rama">"<span class="Y0NH2b CLPzrc">The Ramans do everything in threes</span>"</a>

Also, it will, hopefully, give us that kind of redundancy we didn't have last time. Last time we had 'preferred' rover (that worked well), one that might be used as replacement, and the 'old' one (with hardware and software lagging). This time all three are up the to job.
<h2>Software</h2>
Pyros worked. Worked well in well locally and in school environment. We were able to use and code for rovers from Idle (basic Python IDE) on school computers as well as Unix based OSes (Linux/OSX). There were some issues with message throughput - bugs that were fixed as we have gone along. That particular bug was about limiting number of messages that can be serviced in one iteration of agent's or service's loop. Limit still exists but at least is much higher and we can consider it as 'known problem with a workaround'. And it allowed us to streamline and optimise some other aspects (sending one message for all wheels' speed and orientation instead of 4 for speed and 4 for orientation 50 times a second).

It matured as we progressed through preparation for the PiWars and now we can set up WiFi details through Pyros, read messaging statistic, read storage and the fanciest newcomer - auto discovery rovers using broadcast UDP packets. All our client programs now autodetect existing rovers on the network. How many teams have their rovers discovered like that?

Speaking of client software - a few '<a href="https://gccpiwars.wordpress.com/2018/03/01/this-years-distraction/">distraction</a>' weekends gave us a new look for our client apps:

<img class="alignnone size-full wp-image-1256" src="/2018/02/accelerometer.png" alt="accelerometer" width="800" height="816">

Or blue background and flashy logo in right corner:

<img class="alignnone size-full wp-image-1305" src="/2018/04/flashy-logo.gif" alt="flashy-logo" width="595" height="270">

But the most interesting was OpenCV. Shame we didn't start sooner with it. It is completely new area of hours of fun with computer vision and image processing. At first it seemed quite scary and complex, but splitting image to HSV components and analysing them separately, finding contours, finding contours' properties as area and diameter, applying them as masks to hue channel and doing histograms, drawing 'debug' pictures and shapes - all of it on its own was worth going to PiWars.

Also, it is worth mentioning that we managed some of the stuff we failed to implement last time. For instance - making lunge attack and orbiting around opponent.

When talking of <a href="https://gccpiwars.wordpress.com/2018/03/30/virtual-pinoon/">distractions</a> - this was the something completely different and yet PiWars related. Our club member David did <a href="http://www.unleadedsnail.com/piwars/">online PiNoon interactive game</a>!

<a href="http://www.unleadedsnail.com/piwars/"><img class="alignnone size-full wp-image-1270" src="/2018/03/virtualarena4.png" alt="VirtualArena4" width="3360" height="2098"></a>

Given more time I am sure it will grow to be properly online so people can battle one against another (not only on the same computer as it is currently).
<h2>Hardware</h2>
This time we didn't do as much as last time - our rovers were already built and ready (more or less). Making golf ball catcher and minor improvements to existing bits and pieces (PiNoon holder and VL53L0X sensors holder) were less exciting in comparison to the <a href="https://gccpiwars.wordpress.com/2018/04/10/light-armoured-mobile-nerf-cannon/">Nerf gun</a>! It required engineering (and we again used Sketchup for our designs - luckily provided on the school computers, too). Also it was really nice seeing frowns on some student faces not understanding how two rotating cylinders can do the job - to broad smiles when we finally spun it up. I am sure the moment we go through <a href="https://piwars.org/2018-competition/challenges/the-duck-shoot/">The Duck Shoot</a> challenge there'll be unapproved software tinkering to increase motor speeds - just for fun! Oh, and I hope we didn't leave a lot of mess behind us in the classrooms we used for our club.

Same as for the previous PiWars we did lots of 3D printing, too. CEL's <a href="http://www.cel-robox.com/">Robox</a> 3D printer was put through the test. I am sure I did a few hundred of hours of printing for us. Last year dual material head developed problem which was, after all, quickly sorted out by CEL's engineer. So this time we had a spare head. And for a reason (same old head developed same old problem this time, too). Also, ability to print two materials next to each other (flexible ninja flex and PLA/ABS) helped some <a href="https://gccpiwars.wordpress.com/2018/04/10/light-armoured-mobile-nerf-cannon/">things</a>...
<h2>Support And Sponsors</h2>
Same as last year, it is worth mentioning that our club had support from a few of companies:

<img class="  wp-image-1312 alignleft" src="/2018/04/polo-shirts-2018.jpg" alt="polo-shirts-2018" width="168" height="224">

<a href="http://www.vectric.com/">Ve<img class="  wp-image-876 alignright" src="/2017/03/vectric-logo.png" alt="vectric-logo" width="163" height="127">ctric</a> originally sponsored one rover (it is still going!) and this time provided our team with T-shirts for the day. Also, we always new if we needed any CNC and/or 3D printing we would go to them!

 

<img class="  wp-image-869 alignright" src="/2017/03/blackpepperlogo.jpg" alt="BlackPepperLogo" width="153" height="115"><a href="https://blackpepper.co.uk/">Black Pepper</a> sponsored other rover last time (now completely upgraded) and gave us all the needed support this time (even pledging money or parts when needed). Black stress balls they passed over worked as holders for Somewhere Over the Rainbow balls. And we shouldn't forget the famous 'Rover Calibration Unit' they paid to be printed:

[gallery ids="1311,1310" type="rectangular"]

<img class="  wp-image-864 alignleft" src="/2017/03/creative-sphere.png" alt="creative-sphere" width="112" height="90">

And least but not least, <a href="http://www.creative-sphere.com/">Creative Sphere</a> funded 3D printing (filament and otherwise) and some small bits and pieces (for instance extra servos/ESCs, ATmega328p chip for ultrasonic sensors or upgrade of 9 axis sensor mpu9250).

Aside of those companies, we must mention some parents for their support - especially Mujeeb Parambath that supported us not only morally but materially donating money for another couple of distance sensors and PS3 bluetooth controller.
<h2>Lessons Learned</h2>
Don't leave the hard parts for last. Do computer vision sooner (because it is fun). Rewrite code more often (is it in Agile manifesto or close to it?). Start preparations before Christmas - not after.

But not all lessons were on our expense. For instance - always have spare parts (read: servos) worked well as we have broken a couple in a process of practising for the PiNoon challenge. Have options open - we decided to stick with infrared ToF sensors over ultrasonic due to extra time we needed to make them work reliably in the first place.

See you all on Saturday!</body></html>