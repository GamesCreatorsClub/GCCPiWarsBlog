<html><body><a href="https://piwars.org/2018-competition/challenges/the-minimal-maze/">The Minimal Maze</a> challenge was one we did first last year, ahead of all other competitors, on the day last year and in two goes we did relatively well. Of two goes one was clean run and one was abandoned due forgetting the rules in our excitement (we could have saved it and lost some points but score much more).

This year we left the preparation for the challenge as last. And you'll understand in a minute why!
<!-- TEASER_END -->
Previously we did it using one ToF sensor (VL53L0X) attached to the servo and starting at 45º to left. The idea was to scan distance from the side and front. And it worked well - rover was going through the corners quite nicely (aside of occasional overshooting or crashing straight on). It was funny watching it avoid the walls at the last possible moment! When the sensor is to detect a sudden opening in the left wall, it would switch to three steps:
<ol>
	<li>turn sensor to 90º - directly to the wall and wait it pass past rover</li>
	<li>turn 180º (not really sophisticated as it was time based)</li>
	<li>turn sensor to the right (at 45º again)</li>
</ol>
Now we have two sensors and, again here is picture of their arrangements:

<img class="  wp-image-1113 aligncenter" src="/2018/01/sensors2.png" alt="sensors2" width="552" height="287">Left and right orientations are exactly what we needed this time.

Also, from Somewhere Over the Rainbow challenge, we have following the wall algorithm which was ever so slightly changed here. Instead of stopping when front sensor detected getting close to the wall, we will do turning - away from the wall. Condition for it would be distance of the front sensor to the wall (taking in consideration measured speed as delta offset of previous and current reading of the front distance).

That way our rover can just stick to the left wall, turning to the right, away from it when too close, until it gets to the end. This condition is very similar as in previous year's algorithm: when distance between rover and left wall becomes greater than corridor width(*), that means we are in the middle of a turning and we can then just simply switch from hugging left wall to following right wall and continue until out of the maze. Simple, isn't it?

(*) Corridor width is important and is measured before the run. Sensor scans left and right distances and adds them up to calculate corridor width and then halves it to get 'ideal distance' from a wall (left or right).

This time the simplicity of algorithm won over all other ideas. Well, unfortunately there are still lots of 'moving' parts and gains and noisiness of distance sensors so we cannot still run through the maze at the full speed. If we had another three-four weeks...

{{% media url="https://www.youtube.com/watch?v=oba4mx1OFuc" %}}
</body></html>