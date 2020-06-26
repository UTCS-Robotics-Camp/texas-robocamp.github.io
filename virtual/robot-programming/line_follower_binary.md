---
title: "The Line Follower"
tags: [Binary]
keywords:
sidebar: virtual 
permalink: line_follower.html
---

In this tutorial, we're going to do quite a lot, and it's going to be challenging. We've given you quite a lot of direction here, so you should be able to figure out how to do it. You're going to need a lot of help from the counselors. This is the hardest program you've written so far this camp. It will be challenging, but that's okay! This is how you learn.

We're going to tell you what this program does so you can see where this is all going.

The first program that you will write will allow you to see the numbers returned by the line follower. It lets you explore the raw, numerical data that the device sees, and how each number is in a position corresponding to the line. What I've said will make sense once you've tried it out.

**TODO** *Still not sure what to do about this paragraph. Do we explain the abstraction going on here?*

The second program that you write will allow you to **threshold** the data coming off of the line follower, to determine what is a line and what is not. It will use the LCD on the robot to print asterisks wherever the line is under the line follower, and dashes wherever there isn't.

That will look something like this.

`----**----------`

If it looks like that, there's a line most of the way to the left of the robot.

`**--------------`

If it looks like that, there's a line all of the way to the right of the robot.

`--------------**`

Under the line that looks like that, it will print the number that is the value of the threshold. You will learn about thresholds in this exercise.

Our program says this right now.

`----**----------`

`10`

Just kidding. We want you to find the threshold for yourself. Ours is much bigger than 10.

## Back to the Robot Documentation

The `Robot` class still has a significant amount of functionality that we haven't yet touched, one of those is the use of the analog-to-digital converter

### `int readAdc(int sensorNum)`

On our real robot, a line sensor has an 8-channel analog to digital converter on it. This allows us to hook electronics components up to the robot and read information off of them. The data comes in as analog data; that is to say, electrical voltages. An **analog-to-digital (ADC)** converter converts these voltages to digital information, allowing us to use it in our computer programs.

{{ site.data.alerts.tip }}
<ul>
<li>You can think of the analog data sort of like a dimmer switch on a light. More electricity makes the light brighter. The problem for the microcontroller is that it doesn't work with voltages as analog circuits do. It works with binary, discrete representations. The <b>analog-to-digital (ADC)</b> will convert the voltage into a number that can be understood by the computer.</li>
</ul>
{{ site.data.alerts.end }}

For the purposes of our simulation, we actually use a camera to visualize the ground, and then split up the image into eight separate greyscale rectangles - one for each index of the line sensor.


## The Line Follower Device
**TODO** *If we want them to see the line follower, we're gonna need to make the camera a different color or something, right now it's just a little grey cube (I think)*

Look under your robot at the line follower. **TODO** *At some point we need to explain basic usage of Gazebo* It has 8 little black blocks on it. These blocks are used to detect how much light us coming up off the ground to the robot.

Each of those blocks is hooked up to a separate channel of the ADC. Consequently, the ADC will read each of them and return a value between 0-1023 *is this still true?*.

## Exercise 6.1.1

### The Line Follower Test Target

- Launch the target world.

### Program the Exercise

For this exercise we want you to read the values off of the line follower, while running it over the line target. You're going to need to write a short program to do this.

Here's what your program should do.
- Read each of the values from the line follower using the `readAdc` function(s).
- Print each value ~~onto the USB port using the `Serial.print` functions.~~
- Print a space following each value from the line follower.
- Print a new line at the end of the 8 values.

**TODO** *How do we want them to print? We could just go on the LCD but that might clutter the whole thing?*

When you hook this up, the numbers are going to scream past on the screen really quickly, so we suggest adding a `sleep` after the for loop to make everything much easier to read.


*I think this is still true?*

{{ site.data.alerts.tip }}
<ul>
<li>
The easiest way to implement this is with a for loop and the `int readAdc(byte)` function.
</li>
<li>Count from 0 to 7 using the for loop.</li>
<li>Inside the loop, read the value in the corresponding ADC channel.</li>
<li> <strike>`Serial.print`</strike> <em>cout</em> that value and a space.</li>
<li>After the for loop, use <strike>`Serial.println`.</strike> <em>cout << endl</em></li>
</ul>
{{ site.data.alerts.end }}


### Try the Line Follower

- Move the robot around under the line target. **TODO** *explain how to do this*
- You should notice that the numbers where the black line is present are different from the numbers where there is no black.
  - How are they different?

{% include callout_red_cup.html task="[Exercise 6.1.1]" %}

## Thresholding

If you did the last exercise properly, you should have noticed that ABOVE some value, you could assure that the number being returned by the line follower was the the line being picked up. Beneath some value, what you saw was the white paper reflecting light back.

The difference between the region where the line was and the region where there was no line is called a **threshold**. We are going to use a threshold to determine where the line is.

{{ site.data.alerts.tip }}
Also, weirdly, the line follower seems to pick up the shadows of the wheels. Don't worry about it, and simply realize that even when you account for the wheels, the threshold is still higher than the value for the shadow.
{{ site.data.alerts.end }}

Uh-oh! We're going to have you follow a line on a racetrack, but the lighting may be different! What we'll do to account for this is write a small user interface where you can tune your threshold. While we're at it, we'll also experiment with the line follower and the LCD, and learn a bit about strings in C++.

## Exercise 6.1.2

The next few exercises will build up to a small user interface that will allow you to see the line the way that your robot sees it!

### In C++, Most Text is Just Special Integers
- Write a quick function, call it `void printAsterisks()`.
- Have `loop` call `printAsterisks`.

```cpp
void printAsterisks(BnrOneA robot) {

}

int main(int argc, char **argv) {
    
    ros::init(argc, argv, "robot");

    BnrOneA bot;

    while(ros::ok()) {

        printAsterisks(bot);
        ros::spinOnce();
        
    }

    return 0;
}
```

{{ site.data.alerts.tip }}
It's going to be tempting to put a <code>sleep</code> in here somewhere. Do yourself a favor and resist that urge. Your program will work better and look better if you avoid this.
{{ site.data.alerts.end }}

There's a type that we didn't tell you about way back in ["Simple Math and User Input"](/simple_math_user_input.html) called `char`.

`char` stands for **character**.
Each `char` is 1 byte, meaning that it can represent 256 different numbers.
Each `char` is `unsigned`, meaning that it cannot be negative.
So, a `char` is a number from [0,255] which is used to represent a character.

One way to represent text is called a C string. This is different from a C++ `string`. A C string is an **array** of type `char`. 

*I feel like this is a major detail that students might just gloss over. May want to elaborate more?*

Every number from 0-255 is associated with a unique, printable character. This is called <b>ASCII</b>.

{{ site.data.alerts.tip }}
<ul>
<li>ASCII - American Standard Code for Information Interchange</li>
<li>There's a good article on Wikipedia about learning ASCII if you really want to, but it won't be especially relevant to what we're doing here, so, save that for after camp!</li>
</ul>
{{ site.data.alerts.end }}

- Remember the `bot.lcd1` command? Use that in your program to print 2 asterisks ("*") on the top line of the LCD on your robot.
- Now try it using a C string.
  - Make a **global** variable to store your C string in. Call it `lineUI`.
  - Make `lineUI` 17 chars long.
  - Make a `for` loop to fill `lineUI`.

### Global Variables

You're about to see your first example of a **global variable**. We've mentioned the concept of **scoping** a few times already, which is where one can access a particular variable. A variable that is global can be seen *everywhere* in the file. 

In practice, we try not to make too many variables global, because this can lead to confusing behavior in more complicated programs. Sometimes, however, we want every part of our program to have access to a variable. In these cases, we choose to make our variables global.

```cpp
char lineUI[16]; //global variable

void printAsterisks(BnrOneA bot) {

  for(int i = 0; i < 16; i++) {
    lineUI[i] = '*';
  }
  lineUI[16] = 0;
  bot.lcd1(lineUI);
}

void loop() {
  printAsterisks();
  bot.lcd2(lineUI); //Notice how we can reference lineUI in both functions!
}
```

- What the heck is '*'?
  - Single quotes surround a **character-literal** in C.
  - Just like a string can be surrounded in double-quotes (which is called a **string-literal**), a single character can be surrounded in single-quotes.
- You probably also noticed that I set `lineUI[16] = 0;`
  - C strings are **null terminated**. This means that you put a 0 at the end of the string to mean that the string is done.

{{ site.data.alerts.tip }}
Null termination allows us to have shorter or longer strings stored in the same memory. This means that if we only wanted lineUI to be 8 characters long, we'd just put a zero in `lineUI[8]`.
{{ site.data.alerts.end }}

- Go ahead and run the program from the example above. Don't forget to add any declarations and things that I didn't put into the code snippet!

### Modify the `for` Loop from above

In our UI, what we want is for the top line to show us where the line is. Eventually, there will be asterisks where the line is, and dashes everywhere else.

However, we run into a problem. There are 8 sensors on the line sensor, and 16 characters on the top line. So, we'll want 2 asterisks for every sensor on the line follower. If we have that, and ONLY asterisks turned on for each sensor on the line follower that sees a part of the line, then we will be able to see the line the way that the robot sees it!

- Modify the `for` loop from the example above to **iterate** through the loop 8 times so that only 8 asterisks show up on the LCD.
  - Don't forget to change your null terminator.
  - Your for loop should now count from 0-7. `int i = 0; i < 8`!
- Modify the `for` loop so that it only iterates 8 times, but still prints 16 characters.
  - Do this by making every time it iterates through the loop insert 2 asterisks into `lineUI`.
  - You can do this by multiplying `i * 2` when you fill in `lineUI`
  - That, however, will skip ever other entry in `lineUI`, so, make it also fill in `i * 2 + 1` with an asterisk every time it iterates through the `for` loop.
  - Try it out!

{{ site.data.alerts.tip }}
If these steps don't make sense, try just printing out the values of <code>i*2</code> and <code>i*2+1</code>. You should notice a pattern!
{{ site.data.alerts.end }}

{% include callout_red_cup.html task="[Exercise 6.1.2]" %}

## Exercise 6.1.3

Now we're going to write another UI. This should all go into your current program.

- Add a new global variable, `double thresh`, and set it to zero.
- In `printAsterisks` print the value of `thresh` on line 2 of the robot's LCD.
- Add some code to the loop block that responds to button presses. 
  - When button 1 is pushed, add .01 to `thresh`.
  - When button 2 is pushed, subtract .01 from `thresh`.
- Try it out. The number on the second line should rise when pressing the top button and drop when pressing the lower button.

{% include callout_red_cup.html task="[Exercise 6.1.3]" %}

## Exercise 6.1.4

Now we're going to make it so you only see where the line is under the line follower, allowing you to see the line the way that your robot does.

- Make a global array of type `int` with 8 elements. Name it `adc`.
- Modify the loop so that it reads the ADC using `one.readAdc`.
  - Use a `for` loop so that it fills the `adc` variable.
- Modify the `for` loop in `printAsterisks`.
  - Every time `adc[i] > thresh`, it should put 2 asterisks.
  - Every time `adc[i]` is not `> thresh`, it should put 2 dashes.
- Run your program. Put the line target under the line follower. Push the up button until there are only asterisks under the line follower sensors that are over the line, and dashes everywhere else.
- Move the target around and see how the robot can now "see" the line.
  - Ideally, you only see the line in one place on the LCD, and it is only 2 asterisks wide.

{% include callout_red_cup.html task="[Exercise 6.1.4]" %}


## Next Step

Proceed to ["Line Following"](/line_following.html)