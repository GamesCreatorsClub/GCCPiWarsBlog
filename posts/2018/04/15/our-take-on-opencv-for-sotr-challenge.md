<html><body><p> 

OpenCV is fun. It looked scary before we tried it but when we did, it turned out to be much easier than we anticipated. Shame we didn't start with it sooner (and by sooner I mean for last year's competition).

Our rovers were equipped with Raspberry Pi cameras since day one. The idea was to use them for follow the line challenge, for recording and first person driving - none of which really worked well due to lack of time to spend on it. Now, for the <a href="https://piwars.org/2018-competition/challenges/somewhere-over-the-rainbow/">Somewhere Over the Rainbow challenge</a>, we finally made a use of it!

https://www.youtube.com/watch?v=G6VXJVNCVX8
</p><h2>Setting Up the Picture</h2>
We read a few tutorials online and decided to go with an HSV picture as a base for image analysis. Our rovers have a camera service that delivers 'raw' byte data of an image in RGB format directly from the camera and delivers it to all interested parties over MQTT. That allows us not only to break the code to smaller chunks and make services where code provides access to hardware or software resources, but also to easily implement a monitor of what is really happening to the rover at any time.

So, as soon as we receive image we prepare it to be used in OpenCV:

```python
, False)

value = numpy.argmax(hist)

if value  145:
  return "red", value
elif 19 &lt;= value &lt;= 34:
  return "yellow", value
elif 40 &lt;= value &lt;= 76:
  return "green", value
elif 90 &lt;= value &lt;= 138:
  return "blue", value
else:
  return "", value

```

Here it is when colour is spotted and mask applied to the hue channel:<img class="alignnone size-full wp-image-1297" src="/2018/04/greenfinal.png" alt="GreenFinal" width="1136" height="934">The left image is of the found contour, middle of mask applied to the hue channel (see above what hue was looking like as complete) and last image is the result... well, for looks!

The rest is for the main Somewhere Over the Rainbow agent to process the recognised colours. When there is only one we take it as it is. When there are more than one coloured balls recognised we check if any of them are red and if so discard them as they are usually mainly from the noise of the background. If still undetermined - we take more pictures and process more. Speaking of red - red and yellow we take multiple takes of reading picture as camera's adaptive lighting can change over several frames and produce better results. For green and blue this are far more deterministic...

Here it is when all was put together:

https://www.youtube.com/watch?v=1LdJv2Xz3A8

 </body></html>