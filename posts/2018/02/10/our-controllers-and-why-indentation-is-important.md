<html><body><p>To control our rover we have our standard controllers. Last year we used a modified ps2 controller with a PiZeroW inside. This enabled us to remotely send packets through the WiFi. This was a relatively easy way of remotely driving the rover. However, it had some flaws. Firstly, because it used the WiFi hotspot that we had to carry around, it meant that the TCP packets would have to first be sent to the hotspot over WiFi, then to the rover over WiFi, and data was sent back through the same path. With lots of other 2.4GHz traffic on the spot at times we had latency of up to second or two!

<img class="  wp-image-1166 aligncenter" src="/2018/02/piwarscontrollersetupold.png" alt="PiWarsControllerSetupOld" width="527" height="280">

So, this year we are planning to cut the corner, and connect a controller directly to the rover.

<img class="  wp-image-1165 aligncenter" src="/2018/02/piwarscontrollersetupnew.png" alt="PiWarsControllerSetupNew" width="385" height="299">
<!-- TEASER_END -->
The way we will do this is with a (knockoff) PS3 controller, connected via Bluetooth to the rover. This is way better because there would be far less points in the packets route, and because its more direct, it should have a shorter travel time, meaning less delay. Also, there is no three way acknowledgement TCP robustness relies on. YAY!

<img class="  wp-image-1164 aligncenter" src="/2018/02/piwarscontrollersetupoldsoftware.png" alt="PiWarsControllerSetupOldSoftware" width="440" height="247">

So we set to work on coding this. First we had to connect controller to (the pair to) the Raspberry Pi. Then we, needed to make some code to actually utilise the controller connected in /dev/input/js0; the place that any Bluetooth/USB/Wired controller would connect to. Because on the modified PS2 controller with a PiZeroW we connected the controller inputs on the RPi in a similar way, to still appear as a controller on /dev/input/js0, we could easily just transfer the code. All that was needed was to knock off a few lines to disable the s1306 screen, because they were not needed, and just patch up the code to work with this controller.

<img class="  wp-image-1163 aligncenter" src="/2018/02/piwarscontrollersetupnewsoftware.png" alt="PiWarsControllerSetupNewSoftware" width="479" height="312">

All was working, but we noticed a problem. All the inputs were really delayed, with the delay increasing by the minute. Something wasn't right.
</p><h2>Silly Problem</h2>
Then we realised the problem. Because we still used PyROS to send packets internally in the rover (which was instantaneous) we needed to loop the controller service's thread, to process keys, buttons and sticks. This meant that we were sending packets at around 50 times a second, so we thought. However there was a little problem in PyROS's code. PyROS would wait a set amount of time before executing the next step, and it would achieve keeping this timing with a loop, waiting for the right time to strike. In the code below you can see our mistake. While PyROS was waiting for the next time to run the code's processes (to read the inputs) it would constantly read them anyway. This meant that we were sending packets at around 5000 times a second.

```python

def loop(deltaTime, inner=None):
    for it in range(0, int(deltaTime / 0.002)):
        time.sleep(0.0015)
        if client is None:
            time.sleep(0.0005)
        else:
            client.loop(0.0005)

    if inner is not None:
        inner()

```

Because of it 'inner' code was executed with total delay of 2ms instead of originally expected 20ms and thus spamming drive with messages (or just stop). The drive service managed to process 10 times the volume of messages, but was giving four to eight times that much messages for the wheels service (one or two message per wheel - one for position and one for speed) which at peaks produced 400 extra messages per second. This meant that the wheels service would clog up with messages to execute, and not be able to execute them in the queue in time, until it has process the other hundreds of useless messages. A bit of a silly mistake there!</body></html>