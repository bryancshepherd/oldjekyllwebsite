---
layout: post
title: Physical Computing - Puppies VS Kittens on Reddit
date: 2015-12-28
author: bryan
comments: true
categories: [Articles]
noindex: false
published: false
---

"The best way to have a good idea is to have a lot of ideas." - [Linus Pauling](https://en.wikipedia.org/wiki/Linus_Pauling)

#### Physical Computing
Although the term is used in a lot of ways, [physical computing](https://en.wikipedia.org/wiki/Physical_computing) usually refers to using software to monitor and process analog inputs and then use that data to control mechanical processes in the real world. It's already commonplace in some areas (e.g., autopilots), but it will be all the rage as the Internet of Things grows and automates. It's also often used in interactive art and museum exhibits like the [Soundspace exhibit at the Museum of Life and Sciences in Durham, NC](http://lifeandscience.org/exhibits/?exhibitid=1301). In this case we're manipulating the brightness of two LEDs based on the popularity of animals on Reddit, which I'd say is closer to the art end of the spectrum than the autopilot end.

#### RPis
Rasberry Pis are great for generating ideas. Because they consume very little power and have a very small form factor, they almost beg you to think of tasks that you want them to get started on and then shove them away in a corner for a while to work on. Because they run a modified version of Debian, it's easy to take advantage of things like the Apache webstack and Python to get things up and running quickly. In fact, in a previous post, I showed an example of using a Pi to fetch data and serve it to a simple D3-based dashboard.

This is another project that takes advantage of the Reddit API, Python on the RPi, and the GPIO interface on the RPi to visually answer the age old question "What's more popular on the internet - puppies or kittens?"

#### Get data via the Reddit API

Reddit has a very easy to use API, especially in combination with the Python PRAW module, so I use it for a lot of little projects. On top of the easy interface, Reddit is a very popular website, ergo lots of data. For this project I used Python to access the Reddit API and grab frequency counts for mentions of puppies and kittens. As you can see in the code ([GitHub repo](https://github.com/bryancshepherd/PuppiesVSKittens)), I actually used a few canine and feline related terms, but 'puppies' and 'kittens' are where the interest and 'aww' factor are, so I'm sticking with that for the title.

The PRAW module does all the work getting the comments. After installing the module all that's required is three lines of code:

```python
import PRAW
r = praw.Reddit('Term tracker by u/mechanicalreddit') # Change 'yourname' to your Reddit username
allComments = r.get_comments('all', limit=750) # The maximum number of comments that can be fetched at a time is 1000
```
You now have a lazy generator (`allComments`) that you can work with to pull comment details. After fetching the comments, tokenizing, and a few other details that you can look at in the Git repo, we have a list of tokens (`resultsSet`) that we can send to a function that keeps a running sum for each set of terms:

```python
def countAndSumThings(resultsSet, currentCounts):
    resultsSet = resultsSet.lower()
    for thing in currentCounts:
        thingLower = thing.lower()
        searchThing = ' '+thingLower+' '
        thingCount = resultsSet.count(searchThing)
        currentCounts[thing]+=thingCount
    return currentCounts
```

#### Visualizing the data - i.e., Little Blinky Things

Setting up the RPi GPIO circuitry to control the LEDs is beyond the scope of this post, and it's also covered better elsewhere than I could do. Here are a couple of resources you may find helpful:

- http://www.instructables.com/id/Easiest-Raspberry-Pi-GPIO-LED-Project-Ever/
- http://www.thirdeyevis.com/pi-page-2.php
- http://raspi.tv/2013/how-to-use-soft-pwm-in-rpi-gpio-pt-2-led-dimming-and-motor-speed-control

On the software side, the RPi.GPIO module provides the basic functionality. The first part of this code initializes the hardware and prepares it to handle the last two lines which set the brightness of the LEDs.

```python
# The RPi/Python/LED interface
import RPi.GPIO as GPIO ## Import GPIO library
GPIO.setmode(GPIO.BCM) ## Use internal pin numbering

# Initialize LEDs
# Green light
GPIO.setup(22, GPIO.OUT) ## Setup GPIO pin 25 to OUT

# Initialize Pulse width modulation on GPIO 25. Frequency=100Hz and OFF
pG = GPIO.PWM(22, 100)
pG.start(0)

# Red light
GPIO.setup(25, GPIO.OUT) ## Setup GPIO pin 22 to OUT

# Initialize Pulse width modulation on GPIO 22. Frequency=100Hz and OFF
pR = GPIO.PWM(25, 100)
pR.start(0)

# Skip some intermediary code...(see repo for the details)

# Update lighting
pG.ChangeDutyCycle(greenIntensity)
pR.ChangeDutyCycle(redIntensity)
```

Put this in a loop and you're good to go with continual updating.

And here's a picture of the final setup. I added a cover to disperse the light a little bit and help with some of the issues with visual perception discussed below:

![Blinky Blinky](http://www.bryancshepherd.com/images/led.jpg)

#### Puppies or Kittens?
Drum roll please....

If the Internet is 90% cats, Reddit is an anomaly. Dogs were usually the most popular topic of conversation.

#### Some frustrations with...

###### ...hardware
I didn't have any breadboard jumper wires to connect the LEDs and I had some difficulty finding them. I had some male-to-male jumpers from an Arduino kit, but connecting an RPi to a breadboard requires female-to-male connectors. I expected Radio Shack to have them, but no luck. Incidentally, an old 40-pin IDE connector will also work with the newer 40-pin RPi GPIOs, but not 80-wire connector. Note that while the headers for these both have 40 sockets and will physically fit onto the RPi board, attempting to use an 80-pin cable will almost certainly break your Pi. If you're not sure which type of cable you have, just count the ridges in the cable from the wires. If you still have wires to count after you get to 40, you can't use that cable. Unless, that is, you want to do what I ultimately did and use individual wires from the cable:

![Disassembling an IDE cable](http://www.bryancshepherd.com/images/idecable.jpg)

The individual wires of IDE cables are easy to separate and can be stripped with a tight pinch. Because they are single wires, they are easier to manage than if you tried to work with something like speaker wire.

##### ...the visualization method
Brightness, it turns out, is not to be a great way to indicate magnitude. Because of the way our eyes perceive color and the way that LED brightness is affected by increases in current it's not a very clear comparison. The relationship between LED brightness and current is not linear, which the raw percentages can't be used as intensity levels.

##### ...the data
Reddit gets very busy sometimes, and during those times posting of comments can slow down considerably due to commenters receiving page errors and a comment backlog developing. Presumably every comment eventually gets posted and is available to the API, but I'm not 100% sure. If comprehensiveness was a concern this would need to be looked into in more detail.

#### What's next

In hindsight it would have been better to make the lights blinks faster or slower rather than change the brightness. If there's a version 0.2 of this project, that will be one of the changes.

Currently you have to change the code directly to change the search terms. Although setting up an interactive session or GUI is overkill for this application, it would be pretty easy to have the Reddit bot that does the term searches check its Reddit messages every so often for a list of new search terms. In that case, changing the search terms would be as easy as sending a Reddit message in some predefined format.

And finally, so maybe you don't care about the relative popularity of pets on Reddit. With the upcoming election plugging in the candidate names might be interesting. Now that I have some real breadboard wires, I'll probably set that up the next time I have a free weekend. But go ahead, feel free to clone the repo and do something useful.
