Title: This Year's Distraction - Part III - The Sound
Tags: Home, Blog, piwars
Date: 2019-02-23 21:29
Author: Daniel Sendula
Background-image: /2019/02/odyssey-2001.jpg

# This Year's Distraction - Part III - The Sound

But UI itself is not enough for distraction. Rover can show nice images, we can use screen to temporarily suspend wheels operations (turning and driving), calibrate all, but... It is quite quiet stuff. What if we get tiny amplifier like [this](https://shop.pimoroni.com/products/adafruit-mono-2-5w-class-d-audio-amplifier-pam8302) + tiny speaker that can be fitted just under the top surface of the rover? That would allow our rover having sound as well.

<!-- TEASER_END -->

First is to make sure RPi is sending sound to a GPIO (there are plenty of resources on the internet explaining how to do it). Second thing is to build small filter and wire all together:

![Sound System](/2019/02/sound-system.jpg "Sound System"){ : style="width:100%;"}

Screen service then can gain another MQTT topic next to `screen/image`: `screen/sound`. And with following:

```shell
$ mosquitto_pub -h rover6 -t screen/sound -m alarm_system_keypad.wav
```

we can play arbitrary wav (or ogg - thanks to pygame).

But that's not enough. Right? If rovers are going to gain some intelligence (later, much later, I hope) then we need it to talk, too!

A quick search on the internet produced this:

[http://www.festvox.org/flite/](http://www.festvox.org/flite/) - CMU Flite: a small, fast run time synthesis engine.

It is incredibly easy to install and use:

```shell
$ sudo apt-get install flite
```

```shell
$ flite "Hello!"
```

But original voice wasn't the best. A bit more internet searches and trying out various things produced nice, slightly disconnected - even bored, female voice called 'eey' from here: [http://www.festvox.org/flite/packed/flite-2.0/voices/](http://www.festvox.org/flite/packed/flite-2.0/voices/) ([http://www.festvox.org/flite/packed/flite-2.0/voices/cmu_us_eey.flitevox](http://www.festvox.org/flite/packed/flite-2.0/voices/cmu_us_eey.flitevox))

```shell
$ flite -v -voice /home/pi/cmu_us_eey.flitevox "Shutdown initiated"
```

Also, to add some character and intonation to it we can use 'standard' deviation, too:

```shell
$ flite -v -voice /home/pi/cmu_us_eey.flitevox --setf duration_stretch=1.2 --setf int_f0_target_stddev=70 "Waiting for wheels to stop."
```

Huh. Now we have so many possibilities to give enough personality to our rover that people can get scared of it!