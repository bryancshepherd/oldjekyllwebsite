---
layout: post
title: Physical Computing Project - A Visual Representation of Puppies VS Kittens on Reddit
date: 2015-12-28 
author: bryan
comments: true
categories: [Articles]
noindex: false
published: false
---

"The best way to have a good idea is to have a lot of ideas." - [Linus Pauling](https://en.wikipedia.org/wiki/Linus_Pauling)

Rasberry Pis are great for generating ideas. Because they consume very little power and have a very small form factor, they almost beg you to think of tasks that you want them to get started on and then shove them away in a corner for a while to work on. Because they run a modified version of Debian, it's easy to take advantage of things like the Apache webstack and python to get things up and running quickly. In fact, in a previous post, I described a simple example of using a Pi to fetch data and serve it to a simple D3-based dashboard.

In this post I'll cover another project that takes advantage of the Reddit API, Python on the RPi, and the GPIO interface on the RPi to visually answer the age old question "What's more popular on the internet - puppies or kittens?"

#### Get data via the Reddit API

Reddit has a very easy to use API, especially in combination with the Python PRAW module, so I use it for a lot of little projects. On top of the easy interface, Reddit is a very popular website, ergo lots of data. For this project I used Python to access the Reddit API and grab frequency counts for mentions of puppies and kittens. As you can see in the code (GitHub repo), I acctually used a few canine and feline related terms, but 'puppies' and 'kittens' are where the interest and 'aww' factor is, so I'm sticking with that for the title.

#### Some frustrations

I didn't have any breadboard jumper wires to connect the LEDs and I had some difficulty finding them. I had some male-to-male jumpers from an Arduino kit, but connecting an RPi to a breadboard requires female-to-male connectors. I expected Radio Shack to have them, but no luck. Incidentally, an old 40-pin IDE connector will also work with the newer 40-pin RPi GPIOs, but not an 80-wire connector. While the headers for these both have 40 sockets and will physically fit onto the RPi board, attempting to use an 80-pin cable will almost certainly break your Pi. If you're not sure which type of cable you have, just count the ridges in the cable. If you still have wires to count after you get to 40, you can't use that cable. Unless, that is, you want to do what I ultimately did and use individual wires from the cable. 

The individual wires of IDE cables are easy to separate and can be stripped with a tight pinch. Because they are single wires, they are easier to manage than if you tried to work with something like speaker wire. 

Reddit gets very busy sometimes, and during those times posting of comments can slow down considerably due to commentors receiving page errors and a comment backlog developing. Presumably every comment eventually gets posted and is available to the API, but I'm not 100% sure. 



