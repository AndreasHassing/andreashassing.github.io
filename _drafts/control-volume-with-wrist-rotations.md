---
layout: post
title: "Control Volume with Wrist Rotations"
tags: [Smartwatch, Niche Gimmick, Silly, Gesture, Tizen]
author: Andreas Bjørn Hassing
---

Due to unfortunate circumstances I won't get into, I have been at home the last week taking care of my daughter most of the day - which has been a lot of fun! She sleeps around one and a half hour to three hours for her daily nap.

What in the world do you do with that spare time?

Hint: it's got something to do with my good ol' Samsung Gear S3.

Obviously, I couldn't just sit on my hands or watch Netflix - because who are we kidding, I've done a lot of that already the past few months (COVID, stay indoors, stay safe, protect the predisposed). No; I wanted to get something done, practice and try out new things. This time around I chose to create a volume controller that would respond to a hand gesture like rotating the wrist. Pretty simple, pretty silly, pretty Iron Man-like, pretty niche gimmicky - who cares, it's fun!

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I feel like Iron Man 😄🤖 <a href="https://twitter.com/Sonos?ref_src=twsrc%5Etfw">@sonos</a> <a href="https://twitter.com/hashtag/tizen?src=hash&amp;ref_src=twsrc%5Etfw">#tizen</a> <a href="https://twitter.com/hashtag/samsunggear?src=hash&amp;ref_src=twsrc%5Etfw">#samsunggear</a> <a href="https://t.co/R4fPLigcIP">pic.twitter.com/R4fPLigcIP</a></p>&mdash; Andreas B. Hassing (@AndreasHassing) <a href="https://twitter.com/AndreasHassing/status/1265571056078725125?ref_src=twsrc%5Etfw">May 27, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Today I'll write a little about how I quickly mashed together the ability to do that.

## Step 1: Requirements

- Gestures should only be recognized when in some form of **control-mode** (i.e. it shouldn't raise or decrease the volume when I'm doing jumping jacks).
- I should be able to easily enter **control-mode**.
  - I should not have to touch the screen or dial the bezel to enter **control-mode**.
  - Pressing the <kbd>Home</kbd>-button twice (lower-right) could do. This can easily be configured in the Watch configuration.
- In **control-mode**, I should be able to control the volume of my (Sonos|Alexa|...) speakers by rotating the wrist on which my smartwatch is attached.
  - The volume should be controlled the same whether the watch is attached to the left- or right hand (i.e. no discrimination towards lefties).
- I should be able to exit **control-mode** easily.
  - Pressing the <kbd>Back</kbd>-button (upper-right) could do.

## Step 2: How do you write an app for a (Tizen) smartwatch?

1. Click [here](https://docs.tizen.org/application/dotnet/index).
1. Get Extension for Visual Studio.
1. Code in C#.
1. Compile.
1. ???
1. What is it with these cryptic deployment error messages and close-to-zero documentation on what they mean?
   - > Remember to change Tizen compiler settings in Visual Studio to sign your app, or you can't try it out on your non-emulator watch.
1. ???
1. Profit! 💰

The end. <sub>If it weren't for that horrible startup-time!</sub>

In all seriousness, the .NET application was by far (to me) the easist to write in the shortest amount of time. But - I quickly noticed that the startup-time was horrible; this was a deal-breaker to me since I wanted the startup-time to be blazing fast such that double-pressing Home wouldn't have me waiting for a long time before I could start using the volume controller.

If you don't have a hard requirement for quick startup-time, then using Tizen .NET seems to be great (if you already know C# better than JavaScript/C++).

## Step 2 (let's try again): How do you write an app for a (Tizen) smartwatch which starts in less than a second?

1. To my knowledge, there's no getting around [Tizen Studio](https://developer.tizen.org/ko/development/tizen-studio/download) (unless you already know all the build commands and stuff), so I had to go grab that.
1. If not writing C# (Tizen .NET), I assumed I needed to build the app in C++ since it would most likely have the quickest startup-time.
   - > At this point, I assumed that a Tizen Web application for the watch would have a even-more horrible startup-time than .NET had, so I skipped it at first.
1. Spent a few hours getting a sample to compile and run on my watch.
1. Spent a few hours trying to get the name of a sensor using pretty-darn bad C++ code (it was probably just pretty-darn decent C code in disguise as a `.cpp`-file).
1. Failed doing that and got fairly demotivated at this point - thinking my options were spent.

## Step 3: Go to bed, stop trying and start crying!

_I'm kidding! 😎_

In fact, I was just bummed out at this point and wanted to finally give the Web app a try, with the expectation that I could laugh at how bad the startup-time of _it_ was going to be.

## Step 2 (now we're doing it): How do you write an app for a (Tizen) smartwatch which starts in less than a second and runs JavaScript in a browser(wow?!)?

A funny thing came from that experiment: I was pleasently surprised to see that the Web app started way faster than the .NET app, close to native startup-times it seemed.

Ok great, I know JavaScript fairly well, so let's see what it takes to get stuff done in that.

First of all, avoid the following pitfalls:

- Do not use Classes, they do not work. Instead, use the good old prototype-based approach to object-orientation. If you really, really, _must_ use classes (or a library that uses it), consider using [this Babel plugin](https://babeljs.io/docs/en/babel-plugin-transform-classes).
- Do not use dependencies that depend on `require(...)`, it does not work. If you really, really, _must_ use `require` (or a library that uses it), consider using [Browserify](http://browserify.org/) or [webpack](https://webpack.js.org/).
