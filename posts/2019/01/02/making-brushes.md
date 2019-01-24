Title: Making Brushes
Tags: Home, Blog, brushes, piwars
Date: 2019-01-02 11:08
Author: Daniel Sendula
Background-image: /2019/01/outer-brushes.jpg

# Making Brushes

The last bit that remains for ensuring constant delivery of power to the wheels are brushes. A friend of GCC, practically a team member Saša (he provided us with 3 way sonic control breakout board we, end the end, didn't get chance to use as such - but as breakout board for driving servos for [the nerf gun](http://piwars.abstracthorizon.org/posts/2018/04/10/light-armoured-mobile-nerf-cannon/)) suggested that brushes should follow what we normally see in mechanical relays and real motor brushes: instead of insisting on contact of a flat plate we should provide more of a spherical shape of the brush. That would allow it to overcome tolerance issues we have with 4mm slip ring, 3D printing and such and provide nice contact.

So geared with that idea we came with a solution: a tool that can help us shape 0.3mm x 4mm copper strip to appropriate shape needed - needed not only for spherical 'bump' but rest of it to fit nicely into our 'spring and brush' holders. here's a 'Brush Tool':

![Making a Brush](/2019/01/making-a-brush.jpg "Brush Tool"){ : style="width:100%;"}

<!-- TEASER_END -->

There are 4 inner brushes and 4 outer brushes. Each wheel has two brushes - in case one at one portion of copper ring doesn't have appropriate connection other would 'take over':

![Making Brushes](/2019/01/making-brushes.jpg "Making Brushes"){ : style="width:100%;"}

When the brushes were done, there was another question to answer - a test to be done to ensure we don't need a "Plan B" there as well: is it possible soldering a wire to a brush that is already in place - in a brush holder? Quick test and here it is:

![Brush Example](/2019/01/brush-example.jpg "Brush Example"){ : style="width:100%;"}

Plan B might have been dunking all in a cold water leaving only small amount of copper strip out hoping that water would dissipate heat before it starts the melting plastic it is touching. Fortunately, with enough flux, it was relatively easy to solder it. Also, we would first put a solder to the end of the brush, sand it down back to almost copper, slid it through and re-solder wire. Since there was already some tin on it was much faster to solder wire and not overheat copper strip. 

For brush to to tightly pushed against copper slip ring we needed springs. After some search on the internet, the cheapest option presented itself in quite an unlikely form (and quite wasteful form) - £3 worth of pens:

![Serious Hardware](/2019/01/serious-hardware.jpg "Serious Hardware"){ : style="width:100%;"}

Now we can re-do the inner brushes:

![Inner Brushes Re-done](/2019/01/inner-brushes-redone.jpg "Inner Brushes Re-done"){ : style="width:100%;"}

And outer, too:

![Outer Brush](/2019/01/outer-brushes.jpg "Outer Brush"){ : style="width:100%;"}

The outer brushes holder has two by two brushes - at the same time covers two opposite wheels. THe same thing helps us with amount of wires we need to handle between 'body' and 'top platform':

![Brushes Wired](/2019/01/rover-wiring-inside-2.jpg "Brushes Wired"){ : style="width:100%;"}

Here you can see three set of wires going to one side connector (two opposite inner brushes and one double brush holder). Also, you can see that we've added micro deans connectors to motors - so they can be easily attached to motor controllers. Aside of that there's XT60 connector that is there to connect bottom of the rover (brushes) and the rest of the rover (read: power source). More about wiring in the next post!
