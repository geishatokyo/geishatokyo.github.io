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

The simplest form of animation is transitioning between two states, which have no animation on their own.

First, we'll need an Animator component; the Animator can apply changes to the GameObject it's attached to, or any child of it, keep that in mind when choosing where to put it. The Animator will then require an *Animator Controller*, which you should create in your Assets.

![anim1](/static/images/2014/10/UnityUI/anim1.png)

Now we open the *Animator window*, and click on our newly created Animator Controller. We now need to create all the states we want to use; we'll add transitions to them later, so for now:

![anim2](/static/images/2014/10/UnityUI/anim2.png)

Each state now requires a motion (*Animation*) attached to it, so we create one for each. Even if you intend to use the basic properties of the object in a state (usually the default state), you should still make an empty motion for it, because otherwise you won't be able to define transitions.

![anim3](/static/images/2014/10/UnityUI/anim3.png)

Let's set up out *Animaton*s then; we'll keep the one on the default state empty, but we want some changes in the other one. We open the *Animation window*, then in the Hierarchy we select the GameObject we attached the *Animator* to; in the *Animation window*, in the top left, we select the state we want to change, then simply click the record button (the little red circle); after this point, any changes we make in the editor will be recorded and made part of the animation, indicated by a red highlight on the property.

![anim4](/static/images/2014/10/UnityUI/anim4.png)

For example, you can change the color of an image; when possible, the transition we later create will actually go over time from the previous value to this one, creating a smooth transition.

You can set pretty much any property here, including positions or scale of objects; you can even change if a *Button* is Interactable, which comes in handy if you only want a button clickable in certain states; setting it here means you don't have to bother with it from code.

![anim5](/static/images/2014/10/UnityUI/anim5.png)

With our states now set up, we should set transitions between them. We want to transition using parameters, which we'll set from code, so we should define one first.

![anim6](/static/images/2014/10/UnityUI/anim6.png)

Then, we right-click on each state and set up the appropriate transitions.

![anim7](/static/images/2014/10/UnityUI/anim7.png)

Remember to set the conditions for each transition as well! (At the bottom.)

![anim8](/static/images/2014/10/UnityUI/anim8.png)

If we properly set up a motion for each state, the settings for the transition should now be visible above the conditions. You can set the little highlighted blue area in the middle or slide around the two bars representing the states to fine-tune the time it takes for the transition to complete.

Now all we need to do is call [`Animator.SetBool(string name, bool value)`](http://docs.unity3d.com/ScriptReference/Animator.SetBool.html) from code to change our parameter and we're good to go! Or if we don't want to make a script, we can test it by changing the parameter directly in the *Animator window* during Play mode.

![anim9](/static/images/2014/10/UnityUI/anim9.png)

### Animate within states
