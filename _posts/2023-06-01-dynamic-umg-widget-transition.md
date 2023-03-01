---
layout: post
title: "Dynamic UMG Widget Transition in Unreal Engine"
date: 2022-11-23 13:30:00 +0900
tags: [Unreal, UI]
toc: true
---

If you ever played games from the PS1 or PS2 generation, you may notice certain aesthetics shared among their UI, but also how some of them would have transitions where UI elements dynamically reposition and resizing themselves to adapt to the next screen's layout. This has gone out of fashion in more recent games.

Let's bring them back in Unreal Engine's UMG system!

# Initial brainstorming

*If you want to go straight to the tutorial without the brainstorming ramble BS, skip to the **Implementation** section down below.*

A while ago I was playing **Final Fantasy XIII**. Say whatever you want about the game, I couldn't care less, because for me, the game is great, but what's also got me is how lively the UI is in general. Futuristic vision of the mid 2000s with lots of moving parts and slick transitions, it is a sight to behold. As I open the Crystarium menu, I noticed the party member box moves to the right, and it's visibly expanding to full size of the box if I have 6 party members in the stash. (Unfortunately the copy on my hard drive was wiped as I make space for UE project files. I'll reinstall it once I had larger SSD...) Then I played other games for PS1 and PS2, and noticed similar UI transition also happening there as well.

That kind of transition somehow common on those older games, and I felt like those things are getting less prevalent as we step into current generation of games. So I thought, it should make a comeback, and I want to implement this in Unreal Engine with its UMG system.

There was a snag, though. I built all of my widgets exclusively using [containers](https://joyrok.com/UMG-Layouts-Tips-and-Tricks) as opposed to Canvas Panels, so the elements won't be easily repositioned around like a Canvas Panel would. Render Transforms is a thing, and while it *could* work for repositioning the widget around.

When I asked over on [benui's Discord server](http://discord.benui.ca/) about this old timey UI concept, Nick Darnell, an Epic Games staff who created the UMG system, chimed in and gives me some insight about it:

> It’s definitely much harder, and a big reason those things got scrapped was because they suck for different aspect ratios, and portability.
> 
> As long as you engineer it using invisible point widgets or a similar concept to effectively allow you to procedurally animate them to the right place reliably, or animation using sequencer to a value determined at runtime

Then, it got me. One day I was about to root my Android phone, and as I was bored, I decided to turn on the developer setting that shows all the bounding box of the UI elements. At least in Android 11, swiping down the notification bar reveals more shortcut buttons, with the transition repositioning the buttons to different grid. One thing I noticed as I slowly swipe down to progress through the transition, the invisible slots for the collapsed icons are still there, while the icons moves to the invisible expanded slots. Not to mention, Android is an operating system and expected to work on many resolutions and whatever the user throws at it.

This is the clue that I was looking for to implement this in Unreal Engine's UMG system while still relying on containers and conservatively using Canvas Panel.

# Implementation

> ***NOTE:** Normally I'd handle the tweening on the C++ side of things, but for my tutorials, I'll try to keep it more approachable for everyone and staying with BPs as much as possible.*

With brainstorming ramble out of the way, let's get into actually implementing it in Unreal Engine. Do note that this implementation is not necessarily the cleanest, but works for dynamic positioning dictated by containers (e.g. Vertical/Horizontal Box), and a good starting point for your own implementation.

The heart of this implementation is what I'd like to call **"sleight of widget"** trick - just like sleight of hand card tricks, the gist of it is to create a "decoy" copy of the widget you want to animate on the same size and position as the real one from the first screen, then once the decoy widget is done mimicking the real widget on the second screen, hide it to reveal the real one.

