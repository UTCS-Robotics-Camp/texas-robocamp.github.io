---
title: "Line Following"
tags: [robot programming]
keywords:
last_updated: July 17, 2018
summary:
sidebar: tutorials
permalink: line_following.html
---

## Line Following

In this next step, you'll be writing a program to follow a line. We'll be providing you with some of the outline for figuring out how to do this, but a lot of all this will be on your own.

## Understanding the Sensor
Your car has many sensors on it. The Line Follower is an array of 8 analog infrared sensors, which you will use to follow the line. Bot'n'Roll provides a function `readAdc(byte)` which allows for you to detect the values being reported by each of the sensors. Since there are 8 infrared sensors on the Line Sensor, the permitted values for byte are 0-7. This function will return an `int` between 0 and 1023.

## Linear functions for following lines
There are many ways to attempt to follow a line. The approach we'll be discussing involves defining a linear function for following the line. We'll define a function, `readLine()`, which determines what this value is.

How should this function work? Conceptually, let's imagine that the line is covering every single sensor. This would mean that the robot should keep travelling straight. Now, imagine that only the far left sensor has detected a line. This would mean that the robot should travel towards the left. But what if the line is covering only the sensor to the right of that? We would still want the car to travel left, but we would not want the car to go as far to the left as it would've on the farther sensor.

|0|1|2|3|4|
|:---:|:---:|:---:|:---:|:---:|
|far left|slight left|straight|slight right| far right|

From there, you'll have to think about how to apply that value to each wheel in order to get the desired turn you're looking for. You'll also want to think about what the value for the extremes should be. Are there any particular constraints we have to keep in mind? Also, think about what directions the wheels need to be moved in order to accomplish an even turn. What should be the sign for each? 

## Task X

Now that you've got some ideas on where to start, it's time to begin programming! Remember, the goal is to have thecar follow the line. As one final hint, the only BnrOneA functions you should need to get through this exercise are the `move(speedL,speedR)` and `readAdc(byte)` functions.

{% include cup-callout.html task="X" %}