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

Rasberry Pis are great for generating ideas. Because they consume very little power and have a very small form factor, they almost beg you to think of tasks that you want them to get started on and then shove them away in a corner for a while to work on. Because they run a modified version of Debian, it's easy to take advantage of things like the Apache webstack and Python to get things up and running quickly. In fact, in a previous post, I showed an example of using a Pi to fetch data and serve it to a simple D3-based dashboard.

In this post I'll cover another project that takes advantage of the Reddit API, Python on the RPi, and the GPIO interface on the RPi to visually answer the age old question "What's more popular on the internet - puppies or kittens?"

#### Get data via the Reddit API

Reddit has a very easy to use API, especially in combination with the Python PRAW module, so I use it for a lot of little projects. On top of the easy interface, Reddit is a very popular website, ergo lots of data. For this project I used Python to access the Reddit API and grab frequency counts for mentions of puppies and kittens. As you can see in the code ([GitHub repo] (https://github.com/bryancshepherd/PuppiesVSKittens)), I actually used a few canine and feline related terms, but 'puppies' and 'kittens' are where the interest and 'aww' factor is, so I'm sticking with that for the title.

#### Physical Computing
Physical computing is writing software to control something in the real world. In this case we're manipulating the brightness of two LEDs based on the popularity of our predefined terms on Reddit. It's already commonplace in some areas, but it will be all the rage as the Internet of Things grows and automates.

#### Little Blinky Things

Setting up the RPi GPIO to control the LEDs is beyond the scope of this post. It's also covered better elsewhere than I could do here. Here are a couple of resources you may find helpful:

And here's a picture of the final setup. I added a cover to disperse the light a little bit and help with some of the issues with using this method of visualization dicussed below:

![Blinky Blinky](http://www.bryancshepherd.com/images/led.jpg)

#### Results
If the Internet is 90% cats, Reddit is an anomaly. Dogs were usually the most popular topic of conversation.

#### Some frustrations

I didn't have any breadboard jumper wires to connect the LEDs and I had some difficulty finding them. I had some male-to-male jumpers from an Arduino kit, but connecting an RPi to a breadboard requires female-to-male connectors. I expected Radio Shack to have them, but no luck. Incidentally, an old 40-pin IDE connector will also work with the newer 40-pin RPi GPIOs, but not 80-wire connector. Note that while the headers for these both have 40 sockets and will physically fit onto the RPi board, attempting to use an 80-pin cable will almost certainly break your Pi. If you're not sure which type of cable you have, just count the ridges in the cable from the wires. If you still have wires to count after you get to 40, you can't use that cable. Unless, that is, you want to do what I ultimately did and use individual wires from the cable:

![Disassembling an IDE cable](http://www.bryancshepherd.com/images/idecable.jpg)

The individual wires of IDE cables are easy to separate and can be stripped with a tight pinch. Because they are single wires, they are easier to manage than if you tried to work with something like speaker wire.

Reddit gets very busy sometimes, and during those times posting of comments can slow down considerably due to commenters receiving page errors and a comment backlog developing. Presumably every comment eventually gets posted and is available to the API, but I'm not 100% sure.


#### What's next

Although the LED interface is easy to set up, brightness turned out not to be a great way to indicate magnitude. Because of the way our eyes perceive color and the way that LED brightness is affected by increases in voltage it's not a very clear comparison.

In hindsight it would have been better to make the lights blinks faster or slower rather than change the brightness. If there's a version 0.2 of this project, that will be one of the changes.

Currently you have to change the code directly to change the search terms. Although setting up an interactive session or GUI is overkill for this application, it would be trivial to have the Reddit bot that does the term searches check its email every so often for a list on new search terms. In that case, changing the search terms would be as easy as sending a Reddit message in some predefined format.
