---
title: Unity 4.6 UI Tip&#58 Animation
date: 2014-10-23 19:51 JST
tags:
authors: marconius
---

Intro goes here

The single most obvious change made compared to the way Unity previous did GUI is the fact that every UI element is now a GameObject; compared to the old `OnGUI()` code-based UI, this has the obvious effects of making things significantly easier to oversee, maintain, and to add new elements and set up their relationships precisely; but one happy side effect is that because everything is a GameObject, we can now use an Animator to create UI animations!

This is briefly explained in the [UI Button tutorial video](http://unity3d.com/learn/tutorials/modules/beginner/ui/ui-button), specifically for the case of Buttons, which have a built-in transition system using automatically generated *Animator Controller*s. There's no reason why you can't do the same in the general case, however, and here is how:

## Animate using state transitions

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

## Animate within states
