---
title: Unity 4.6 UI Tip&#58 Override sorting with Canvas
date: 2014-10-23 19:49 JST
tags: unity, unity 4.6, unity beta, ui, unity new ui, canvas, sorting
authors: marconius
---

Intro goes here

When working with 2D spaces, sorting and layering of objects is always a problem; usually in Unity you can do this either using sorting layers, or by setting the Z coordinate of game objects.

The new UI, however, is designed to sort itself based on the transform order of game objects; that is to say, UI objects (in the same canvas) will be drawn based on their order in the Hierarchy tab, with objects being lower being drawn later, on top of earlier ones. This is actually a fairly handy system: you'll be using a lot of UI objects and not having to worry about setting layers or Z coordinates makes things convenient; it does lead to some, at first, counter-intuitive situations like backgrounds being *above* foreground elements in the Hierarchy, but this is easy enough to get used to.

## Overriding

The real problem comes when you involve parent-child relationships in this: you might want object B to be the child of A, because you want, for example, size changes to A to affect B as well. But what if A has a sibling C, which you also want to be drawn under B? Sure you could just move C up in the hierarchy, but C might have a child D you want to draw above A as well... not all that unlikely if A and C are instances of the same prefab!

![canvas1](/static/images/2014/10/UnityUI/canvas1.png)

Here, what we want is for the red child images to be always on top of the parent blue images. But using purely ordering in the Hierarchy this is impossible...

The solution is actually quite simple: all you need to do is add a Canvas component to the child objects; then you just tick the Override Sorting checkbox and make sure the Sort Order parameter is larger than that of your root Canvas.

![canvas2](/static/images/2014/10/UnityUI/canvas2.png)

-----

Created for [Unity 4.6](http://unity3d.com/unity/beta/4.6) beta version 20.

This post is meant for advanced users; for basic information on the new UI features, please check the [offical tutorial videos](http://unity3d.com/learn/tutorials/modules/beginner/ui)!
