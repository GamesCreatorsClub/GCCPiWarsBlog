Title: GCC PiWars 2018
Tags: Home, Blog, nikola
Date: 2018-04-23 19:01
Author: Daniel Sendula
Background-image: /images/rover-holder.jpg

# GCC at PiWars 2018

First apologies for such a delayed post. All the stuff we had to put aside for the PiWars took priority and
slowly this fall behind. But here we are.

![Our pit](/2018/04/2018-PiWars-Our-pit.jpg){ : style="float:right; width:35%; margine-left:10px;"}

In short summary - we've finished 6th, much lower than hoped, but still it is good reflection of all
members hard work. With such luck and errors in judgement we could end up being much further down the list.
But, the most important thing - to have good time getting ready for PiWars and enjoy the even itself
hasn't been spoiled - on contrary. I am sure that day was equally great as last years and we'll try not
to miss next. And hopefully we won't!

<!-- TEASER_END -->

So, without further ado here's what we did on the day:

![Rover On TurnTable](/images/Rover2016-rotation-gif.gif "Rover on Turntable")

## 10:40 Pi Noon Round 1

Now that was first challenge and quite unlucky one. Our 'test' L shaped pin holding rods were slightly lighter and
shorter (2mm instead of 3mm wire and maybe 5cm lower if not more). Previous year we suffered from our rover being
very short and others were able to 'sneak' from behind and pop our balloons reaching across our rover's body.
To overcome that we've extended holder for 20mm ahead and thus made it much harder for someone to reach across
our rover.

Downside was that with heavier and taller balloon holder moved centre of gravity higher (to already too high) and more to the front. Sudden turns would make rover topple over.

![Rover Toppling Over](/2018/04/Rover-Toppling-Over.jpg "Rover Toppling Over")

Also, I think it is fair to say we were the first ones to pop our own balloons in new course with big tower with
spikes in the middle. It is sufficient to say it didn't go well for us. Nerves, spikes everywhere, toppling
rover and we managed to lose from much slower and more shy rover. Better luck next time Naeem!

## 11:10 Straight Line Speed Test

This was, unlike last time, supposed to be one of our best challenges. It was proven that it was challenge
where we ended up last!

![Getting Ready](/2018/04/straight-line-getting-ready.jpg "Getting Ready"){: style="float:right;width:35%;"}

From start it didn't look good. Our rover just swerved to the left - away from the sun. Yes, course was
on the sun and that was first though: VL53L0X sensors do not like direct sunlight and we were realy hit by
it. But, then it continued to swerve to the left even when it went into part of the course that was in
the shade. How bizarre. We soldered on and finished it with many, many penalties but at least gave our best.

![Straight Line](/2018/04/2018-Straight-Line.jpg "Straight Line"){: style="float:clear;width:50%;"}

Oh, last go only fix was to move sensors from 45º/45º to something like 20º/70º and only then it didn't hit
the wall immediately. Almost like right sensor was constantly reporting much bigger distances than left.
And telemetry wasn't really built up to the standard so we didn't get to see any values while attempting
the challenge. That's probably the first lesson we need to learn from 2018 competition: more feedback!

## 11:45 Slightly Deranged Golf

That challenge did expose another problem we have gone to competition with: last minute
commits to git might not work as expected! It was a frantic moment when David and Naeem
took over as soon as we discovered that controller is not really acting as expected - not
all functions of PS3 controller buttons do what we expected. In this instance - ball 'catcher'
(or 'the claw' here) just didn't move. They, in a style of best Hollywood films took over
and after 15 minutes of frantic coding, they commented out all the 'bad' code and fixed it for the
challenge. They wouldn't be able to do anything similar had they didn't write the code
in the first place. Luckily, all was fixed in due time and we proceeded to the challenge area...

But, the claw was spot on:

![The Claw](http://localhost:8000/2018/02/design.png "The Claw")

David's code for controlling the catcher worked perfectly and we had really good results. Time spent
on design sessions, three prototypes down and score! Well done guys! This challenge was exactly as
we expected it to be: really good times in all three goes and the first place overall!

Interestingly enough another team next day had implemented our design for capturing golf ball
and achieved equally good results (second place in pro category).

![Golf](http://localhost:8000/2018/04/golf1.png "Golf")

## 13:10 Somewhere Over The Rainbow

That was another challenge we put quite a lot of effort. From first sessions were we talked about idea how to
start tackling the computer vision and identifying what other problems we need solve,
through those adoptive circle searching algorithms for OpenCV, all way to finding correct algorithms for
traversing arena given differnet combinations of coloured balls. All paid off and our rover did perform
like expected on the day. Well, almost. We needed to do couple of 'restarts' (rescues?) were first
'search' for the corner ended up more to the left than expected. Almost like left sensor did report much bigger
distance than it really was. Otherwise - all coloured balls were detected with precision and rest of traversing
through corners went exactly as coded.

We even asked judges to give us different set of combinations of
balls as we were confident that we can solve any combination. Going three times with exactly the same
combination of colours seemed as a waste of all the effort we did to recognise them correctly.

As we didn't get chance to fine tune PID algorithms that were supposed to make our rover rotate for
exactly 45º, 90º, 135º and 180º - those turnings and similarly finding right distance from the wall
were a bit slower than they could have been. That caused us to be only 2nd overall (of 9 teams that
attempted it), but still very, very good result!

## Interim - Technical Merit

Well, something went really wrong here. We are still stunned by decision that our rover is judged last
after previous year's first place. And, our rover was, after all, one of very few that were designed
from the first principles - not only four motors on a board with Raspberry Pi in the middle.
Janina, David, Naeem, Alex and others really felt let down as it seemed that their design and coding
efforts were not considered at all.

## Interim - Artistic

Here we've got completely opposite situation from Technical Merit. Last minute David and Naeem decided
that they can do something more to the overall 'unattractiveness' of our rover. They wend down really hard
in effort to add at least one more feature to it before artistic merit judging is closed that they've
lost track of time and we completely missed it! But, with some incredible luck, we ended up being
8th of 10 awarded places (sharing it with some other rovers) which wasn't earned at all. Only
thing we can say is 'thank you' to judges who did judge our general artistic skills (or there
lack of) in our absence!

## 14:15 Minimal Maze

Now - there's another challenge which we though we would ace, almost like we did it last time. Oh,
last time lack of knowledge of rules and lack of any help from judge lost us a few points - we could
have rescued our rover and had more successful runs than we originally had. This time we were ready
but at the same time confident that we wouldn't need it at all! Oh, how wrong we were...

After first two corder, which our rover nicely navigated exactly as coded for, it suddenly got 'glitch'
and seen opening on the left (can anyone see the pattern here) where there wasn't one and, falsely
supposed that we are at that part of the maze were we need to switch from following left wall to follow
the right wall. Outcome? It tried to turn back and run for its life. Almost like saying:
"I don't want to be in this maze any more".

![Bad Rover, Go There!](/2018/04/bad-rover-go-there.jpg "Bad Rover, Go There!")

First time we luckily 'rescued' it in the right way and it picked up where it left and successfully
left the maze. Next two times it didn't. Overall we were last. Last of only 4 teams that attempted
it in the first place, so it wasn't that bad for our overwall score, but quite embarrassing.

## 15:10 Duck Shoot

Similarly to the Golf Course, extra fixing of commands was needed, but this time it almost seemed
as routine task.

![Light Armoured Rover](/2018/04/light-armoured-rover.jpg "Light Armoured Rover")

The Duck Shoot was one of the challenges were were aware that there's some skills involved and some
issues with our shooting mechanism (we run out of time to make it better). And targets were really
small and not so easy to be knocked down. Servo wasn't the best mounting place and cannon was
nervously jumping up and down. Unluckily we managed to have so many nerf darts that went
next to the neck (1cm up or down and it would be a hit) or to the base of a duck (so it wouldn't be
strong enough to knock it down).

Overall - another 'unlucky' challenge and we were 13th of 21 teams that gave it a go.

## 16:15 Obstacle Course

![Obstacle Course](/2018/04/Obstacle-Challenge-1.jpg "Obstacle Course")

![Obstacle Course Finish](/2018/04/Obstacle-Challenge-2.jpg "Obstacle Course Finish"){ : style="float:right;width:35%;"}

Well, this challenge didn't go much better than others. Nerves again (David managed to have
rover fall of elevated U section, again!), but of bad luck (one motor wire got unsoldered
while going through pebbles) but overall we did attempt it and finish it. Three motors did
drive rover quite well (and driver was able to quickly adopt to the new situation - after
all our rover can drive forward equally well as backward or sideways). Overall we were
12th of 15 places.

## Blogging

![Blogging](/2018/04/blogging.jpg "Blogging"){ : style="float:left;width:20%;margin-right:10px;"}

Blogging did take at least 1/5th of our effort and did pay off to the extent. We were
third of 12 teams that did blog and helped us in final scoring.

![](/2018/04/dot.gif ""){ : style="float:clear;height:1px;"}

## Quick Analysis, Or What Went Wrong

One phrase can easily summarise it: human factor. In 2017 PiWars we used one VL53L0X sensor.
That sensor went through torture software wise. Unlike many other i2c devices this didn't
have simple protocol but only 'referential implementation' written in C. One of the efforts
we put in that time was to translate it to 'pure' Python and that caused first sensor
we had to go through unscheduled calibration. Or mis-calibration. Result was some unreliable
distances - slightly bigger than what really they were. To fix it, given lack of time,
the easiest thing was to buy another.

Many can already suspect what goes next. For this competition we got two more (as the price
of them went to 1/2 within a year), but when it came to solder it to small PCB to switch
them using XSHUT line, new ones went in first. But second pair (old pair) got soldered
to slightly better, improved PCB and much neater. So, even though first pair was used
in most of our coding, second pair (old pair) ended up on the rover because it looked
neater. Result? Well - the first VL53L0X (mis-calibrated one) ended up as left sensor
and caused rover not to see correct distance on that side. And all three autonomous
challenges depended on reading distances correctly. Well - we failed almost all
(but Somewhere Over The Rainbow) due to left distance was always read incorrectly.

![Neat But Not Good](/2018/04/TwoDistanceSensors.png "Neat But Not Good")

PiNoon is just bad luck - there's absolutely no blame for pilot of the rover at the time,
as is for the Duck Shoot challenge. Even if cannon was more stable we could end up with
similar result. One of the thing was that I insisted of not increasing speed of the
cannon so it doesn't get damaged (and since it was only one it would prevent us
complete the challenge). Slightly faster, stronger hits might have helped but
again there's no way of telling. Obstacle course nerves did affect it (unlike
last time we blamed latency of WiFi) and we lost some precious time for rescue
where we shouldn't have fallen down.

## Results:

But, overall score shows that steady work and perseverance pays off even when
things didn't go well. If we say that in our view we failed at least 3 or 4
challenges and still came 6th of 29 teams! That's really good result!

- The Obstacle Course - 12th
- Slightly Deranged Golf - 1st
- Minimal Maze - 4th (last) of 4
- Straight Line Speed Test - last!
- The Duck Shoot - 13th of 21
- Somewhere Over The Rainbow - 2nd of 9 teams
- Technical Merit - last!
- Artistic Merit - 8th of 10 places
- Blogging - 3rd of 12 teams
- Overall - 6th of 29!

## Final Word

I would like to use opportunity here to thank to all team members for all the
effort - it did pay out! Here we have to mention some that helped the team:

![Straight Line](/2018/04/straight-line.jpeg "Straight Line"){ : style="float:right;width:35%;"}

Alex who due to exams couldn't join us on the day but was responsible for many
design decisions, training of our pilots and special rover stand design; 

Mr Kovacs, a teacher from Kenilworth school who helped club through the year and was
cheering us on the day;

Andrew who implemented sonic sensors breakout board (which due to lack of time we
couldn't incorporate as secondary distance sensors) and contributing to already big code
base for various challenges;

Chris who, similarly to Alex couldn't join us on the day, but was CAD designing
our cannon - the shooting mechanism;

Dorian for being event manager on the day, keeping time and recording the event
with this phone;

Ed and Nick for helping out with the club's day to day business of teaching
games creation fundamentals and generally cheering for us;

Naeem's dad for sponsoring club and family for cheering us on the day;

[Creative Sphere](https://www.creative-sphere.com), [Vectric](https://www.vectric.com) and
[Black Pepper](https://www.blackpepper.co.uk/) companies for sponsoring our club with
bits and pieces needed for our rovers, T-shits for the event and 'special rover stand' (mugs)

[Kenilworth School](http://www.ksn.org.uk/) for allowing club to operate on their premises and lending us
brilliant students

and last but not least Janina, Naeem and David for making PiWars worth doing,
if nothign else but just for watching them learn about engineering and programming
needed for the event like this

and everyone else who contributed to GCC PiWars 2018! Thank you again!