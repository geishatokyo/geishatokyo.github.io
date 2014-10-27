---
title: Unity 4.6 UI System Tips & Tricks
date: 2014-10-23 19:50 JST
tags:
authors: marconius
---

[Unity 4.6](http://unity3d.com/unity/beta/4.6), currently in beta version 20, is introducing an entirely new system for creating user interfaces; although still in beta, the system is already very much usable. Not only that, there is a series of excellent [tutorial videos](http://unity3d.com/learn/tutorials/modules/beginner/ui) that explain the basics of the system very well.

However, as a beta, there are still a number of features that are not fully explained, and in some cases, documentation is outright missing. So with that in mind, here are some tips and tricks you might not be aware based purely on the tutorial videos and documentation. Most of these tips are for fairly advanced issues, so some knowledge of the basic system is recommended before reading on; the very basics of the new UI system won't be explained here.

## Override sorting with Canvas

When working with 2D spaces, sorting and layering of objects is always a problem; usually in Unity you can do this either using sorting layers, or by setting the Z coordinate of game objects.

The new UI, however, is designed to sort itself based on the transform order of game objects; that is to say, UI objects (in the same canvas) will be drawn based on their order in the Hierarchy tab, with objects being lower being drawn later, on top of earlier ones. This is actually a fairly handy system: you'll be using a lot of UI objects and not having to worry about setting layers or Z coordinates makes things convenient; it does lead to some, at first, counter-intuitive situations like backgrounds being *above* foreground elements in the Hierarchy, but this is easy enough to get used to.

### Overriding

The real problem comes when you involve parent-child relationships in this: you might want object B to be the child of A, because you want, for example, size changes to A to affect B as well. But what if A has a sibling C, which you also want to be drawn under B? Sure you could just move C up in the hierarchy, but C might have a child D you want to draw above A as well... not all that unlikely if A and C are instances of the same prefab!

![canvas1](/static/images/2014/10/UnityUI/canvas1.png)

Here, what we want is for the red child images to be always on top of the parent blue images. But using purely ordering in the Hierarchy this is impossible...

The solution is actually quite simple: all you need to do is add a Canvas component to the child objects; then you just tick the Override Sorting checkbox and make sure the Sort Order parameter is larger than that of your root Canvas.

![canvas2](/static/images/2014/10/UnityUI/canvas2.png)

## Reference Resolution

One of the most common problems when designing user interfaces, especially on mobile platforms, is compatibility with multiple display sizes and aspect ratios. Unity UI does allow you to overcome this to some extent by using anchors, but this can lead to unwanted stretching and distortion of images, or simply problematic placement of UI elements when aspect ratio is drastically different. Not setting anchors to stretch, on the other hand, will mean that UI elements maintain their set pixel-size, meaning they will seem very small on a larger display (or very large on a smaller display).

![refres1](/static/images/2014/10/UnityUI/refres1.png)

This issue can be solved through the (currently undocumented) **Reference Resolution** component, attached to your root Canvas, which must be set to Overlay mode. In essence, this component takes Width and Height parameters, then changes the effective pixel size of the Canvas to match these as closely as possible. For example, if you set the parameters to be `800x600` while your actual display is `1600x1200`, in the Editor you will still see the size of the Canvas as `800x600` and will be able to design your UI elements accordingly. When the Canvas is set to Overlay mode, its size will follow the size of your Game screen in the editor, so you can easily test various sizes.

This only works if the aspect ratio isn't changed, however. With different aspect ratios, the Canvas Scale Mode setting comes into play:

- When set to **Expand**, the *shorter* of the width/height of the Canvas's rectangle will be set to be equal to the appropriate parameter; the other one will *expand* to whatever is required to match the aspect ratio of the screen.
- When set to **Shrink**, the *longer* of the width/height of the Canvas's rectangle will be set to be equal to the appropriate parameter; the other one will *shrink* to whatever is required to match the aspect ratio of the screen.
- When set to **MatchWidthOrHeight**, there is an extra parameter, *Match*; setting this to 0 (Width) will make the width of the Canvas match the parameter and stretch or shrink the height to match the screen's aspect ratio; setting it to 1 (Height) will do the opposite; setting it to a value between the two will stretch or shrink both sides to some degree.

### Instantiate issues

There is a small problem with *Reference Resolution*: because there is no method in Unity to Instantiate a GameObject from code already as a child of another object, new objects must be made first, then their `transform.parent` needs to be set. Unfortunately because of the way Canvas objects work, this causes the newly created object to distort, its position, scale or other parameters potentially changing.

The best solution for this issue is to, after setting the `parent`, reset all parameters of the new object to those of its prefab:

	public RectTransform InstantiateUI (RectTransform prefab, Transform parent)
	{
		GameObject newGameObject = Instantiate (prefab) as GameObject;
        newGameObject.transform.parent = parent;
        
        RectTransform newRectTransform = newGameObject.transform as RectTransform;
        newRectTransform.anchoredPosition = prefab.anchoredPosition;
        newRectTransform.anchorMax = prefab.anchorMax;
        newRectTransform.anchorMin = prefab.anchorMin;
        newRectTransform.localRotation = prefab.localRotation;
        newRectTransform.localScale = prefab.localScale;
        newRectTransform.pivot = prefab.pivot;
        newRectTransform.sizeDelta = prefab.sizeDelta;
        
        return newRectTransform;
    }

## Animation

The single most obvious change made compared to the way Unity previous did GUI is the fact that every UI element is now a GameObject; compared to the old `OnGUI()` code-based UI, this has the obvious effects of making things significantly easier to oversee, maintain, and to add new elements and set up their relationships precisely; but one happy side effect is that because everything is a GameObject, we can now use an Animator to create UI animations!

This is briefly explained in the [UI Button tutorial video](http://unity3d.com/learn/tutorials/modules/beginner/ui/ui-button), specifically for the case of Buttons, which have a built-in transition system using automatically generated *Animator Controller*s. There's no reason why you can't do the same in the general case, however, and here is how:

### Animate using state transitions

### Animate within states
