<html><body><p>[gallery ids="25,24" type="rectangular"]

 

In October, we began to plan our rover. We discussed how to steer our rover and considered different ways of steering, for example using tank steering or going with standard car steering.
<!-- TEASER_END -->
Because we couldn't decide on which steering mechanism to use, and since the Raspberry Pi is capable of driving servos, we decided to control each wheel separately. This will allow us to explore different ways of steering and give us greater agility, with the robot being able to drive like a car, turn on one spot and also move sideways.

Also, we discussed which sensors we would need for each Pi Wars challenge. For the Minimal Maze challenge, we realised we had to implement a distance sensor onto the robot. A camera would also be needed to the line challenge. We also agreed on "9 degrees of freedom" sensors (a gyroscope, accelerometer and compass) to control movement more precisely.

Aside of that we needed to come up with a communication system between our rover and controller. Out of all the possibilities we thought that WiFi would give us the greatest range of choices to explore (TCP or UDP sockets, HTTP Rest service, messaging system, etc). That would allow us to use a PC or a mobile phone as a controller for the manually driven rover challenges. And for this we threw another Raspberry Pi 3 to act as a WiFi Access Point and potentially a logger/safety controller from which we would be able to stop the rover.

 </p></body></html>