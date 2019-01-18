Title: Transferring Power To Wheels
Tags: Home, Blog, power, slip ring, rotary transformer, piwars
Date: 2018-10-19 21:32
Author: Daniel Sendula
Background-image: /2018/10/m18-shape-3.jpg

# Transferring Power To Wheels

![Slip Ring](/2018/10/slip-ring.png "Slip Ring"){ : style="margin:20px; width:60%; float:right;"}
For wheels to turn (steer) 360º we cannot have wires going directly to motors and get twisted. Original idea was to go with a ready-made slip ring like this:

It has 6 independent wires were supposed to handle enough current with acceptable resistance but there was one snag: it would need to go directly at the 'Z' axis over the wheel. That wouldn't be an issue if we didn't plan to have absolute positioning which would use AS5600 (digital potentiometer) which requires tiny, button style magnet to occupy exactly the same place - the top of the wheel (wheel arch really) in the dead centre.

## Alternatives

If we are not able to use ready made slip ring there are other options. One is to make our own or to try to pass power in contactlessly. Third option would be to not allow wheel go more than, let's say, 720º; count turns and 'rewind' back when needed. That would, after all, defeat idea of effortlessly steering with 360º freedom.

<!-- TEASER_END -->

## Rotary Transformer

 A device that can allow that is called "Rotary Transformer" and is coming in one of two flavours: axial where windings sit inside each other and "pot core" where windings sit one on 'top' of another:

 ![Pod And Axial Rotary Transformer](https://www.researchgate.net/profile/JPC_Smeets/publication/224195608/figure/fig2/AS:667033284902918@1536044536761/Rotating-transformer-geometries-a-Axial-rotating-transformer-b-pot-core-transformer.png){ : style="width:60%;"}

For such transformer to work we would need to make two coils, make DC to AC circuit and then add rectifier in the wheel hub as well. Not a small feat - especially for such a short time we have to sort out everything else for our rover...

More about that in this really nice [master thesis of Mattia Tosi](http://tesi.cab.unipd.it/45556/1/Mastersthesis_Tosi_Mattia_1035471.pdf). 

## Slip Ring

So, alternative is to make our own slip ring then. Since top of the wheel is taken for button magnet, we can position slip rings around the wheel - in same axis as main, big bearing is sitting. So, the plan is to make two copper strips that go around the wheel hub and use existing brush motor brushes to deliver plus and minus terminals of battery to inside of the wheel hub. We started with 3mm wide and 0.3mm thick slug repellent self adhesive tape. But, on the second thought 0.3mm thick tape is going to be quite thin and possibly rip on the excessive use. Next, slightly more robust solution is 4mm wide and 1mm thick copper plates ordered from the ebay. 1mm thickness is going to ensure life time endurance and Bosh brushes (the cheapest, smallest sold on the ebay again) are 4mm thick anyway so they seemed as a good match.

Design is like follow:

![Wheel Hub Design](/2018/10/wheel-hub-design.png "Wheel Hub Design"){ : style="margin-bottom:20px;margin-top:10px;width:100%;"}

When printed it looks like this:

![Printed Wheel Hub](/2018/10/printed-wheel-hub.jpg "Printed Wheel Hub"){ : style="margin-bottom:20px;margin-top:10px;width:100%;"}

Now, internal tabs are going to be connected to motor controller (H bridge of some kind) for power and to voltage regulator for ATmega controller, nRF24L01 transceiver for communication to RPi and AS5600 as wheel's "rotary encoder" sensor...