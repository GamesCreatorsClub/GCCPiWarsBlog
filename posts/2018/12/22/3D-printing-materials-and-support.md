Title: 3D Printing, Materials and Support
Tags: Home, Blog, 3D Printing, Materials, Support, piwars
Date: 2018-12-22 16:49
Author: Daniel Sendula
Background-image: /2018/12/printing-with-support-1.jpg

# 3D Printing, Materials and Support

PiWars gave us opportunity to play with many different technologies and learn new stuff. 3D printing is one of them.
As our design get more complex they require more thinking and knowledge how to produce good prints. In previous blog
[Transferring Power To Wheels, Part II](http://piwars.abstracthorizon.org/posts/2018/11/10/transferring-file-to-wheel-2/)
we talked about how orientation can help with avoiding support. But sometimes it is not as simple. Similar part again (for holding springs and brushes):

![Spring Holder Design](/2018/12/spring-holder-design.png "Spring Holder Design"){ : style="width:100%;"}

<!-- TEASER_END -->

Now we cannot turn it to some side and still achieve similar quality of print as in previous article nor avoid support inside of the cavities of that model. But luckily my previous visit to [TCT show](https://tctshow.com/tctshow/en/page/home) at NEC got me nice sample of water soluble support material: [BVOH by Fillamentum Industrial](https://www.fillamentumindustrial.com/bvoh).
And this was ideal opportunity to test is, especially as for printing with such support you need at least dual head printer (which my [CEL Robox](http://www.cel-robox.com/roboxdual/) is).

![Printing With Support](/2018/12/printing-with-support-1.jpg "Printing With Support"){ : style="width:100%;"}

It was left for Cura (slicer underneath of Automaker - software for Robox) to decide where and how to add support:

![Print With Support](/2018/12/print-with-support-1.jpg "Print With Support"){ : style="width:48%;  float:left;"}
![Print With Support](/2018/12/print-with-support-2.jpg "Print With Support"){ : style="margin-left:10px; width:48%; float:clear;"}

And when print was done all that was needed was to dunk part into cold water for support to start coming off:

![Support in Bath](/2018/12/support-in-bath.jpg "Support in Bath"){ : style="width:100%;"}

Cleaning after that was very easy as tiny pieces of support that remained on the part was so soft and easy to be pulled off with pair of pliers. If/where it stayed ever so slightly stubborn, some water helped it a lot. So final part (especially cavities) came nice and clean:

![Support Cleaned](/2018/12/support-cleaned.jpg "Support Cleaned"){ : style="width:100%;"}

That is really helpful especially for smaller parts where tolerances are in much worse odds than with bigger parts.

## Epilogue

Sometimes tiny bit of material stays just at the final entrance of a chamber that is heated in the printing head. It is usually very easy to be cleaned with tiny drill and if it still doesn't get out (after gentle rotating drill chopping the plastic) heating drill's tip and re-inserting it would make plastic melt and be easy to pull out and stop obstructing the path.

But, it took several attempts with this material (cool down head after printing, take it off, heat tip of the drill bit, try to get final 1mm of material melt and stick to the drill tip, pull it out, mount head to the printer and try to get material through it) before the most obvious solution came to me: a few drops of _*water*_ down the pipe that delivers filament to the chamber caused material to degrade and stop obstructing path!

It turns out that this material, BVOH, is really easy to work with and seamless remove in water not leaving any residue on the print. Now I'll try to find a way to get more of it!