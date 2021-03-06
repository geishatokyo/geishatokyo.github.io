---
title: Unity 4.6 UI Tip&#58 Canvas Scaler (formerly Reference Resolution)
date: 2014-11-04 19:50 JST
tags: unity, unity 4.6, unity beta, ui, unity new ui, canvas scaler, reference resolution, resolution, scaling
authors: marconius
---

One of the most common problems when designing user interfaces, especially on mobile platforms, is compatibility with multiple display sizes and aspect ratios. The new Unity UI does allow us to overcome this to some extent by using anchors, but this can lead to unwanted stretching and distortion of images, or simply problematic placement of UI elements when aspect ratio is drastically different. Not setting anchors to stretch, on the other hand, will mean that UI elements maintain their set pixel-size, meaning they will seem very small on a larger display (or very large on a smaller display).

![refres1](/static/images/2014/10/UnityUI/refres1.png)

This issue can be solved through the (currently undocumented) **Reference Resolution** component, attached to our root *Canvas*, which must be set to *Overlay* mode. In essence, this component takes *Width* and *Height* parameters, then changes the effective pixel size of the *Canvas* to match these as closely as possible. For example, if you set the parameters to be `800x600` while your actual display is `1600x1200`, in the Editor you will still see the size of the *Canvas* as `800x600` and will be able to design your UI elements accordingly. When the *Canvas* is set to *Overlay* mode, its size will follow the size of your Game screen in the editor, so we can easily test various sizes.

This only works if the aspect ratio isn't changed, however. With different aspect ratios, the **Canvas Scale Mode** setting comes into play:

- When set to **Expand**, the *shorter* of the width/height of the Canvas's rectangle will be set to be equal to the appropriate parameter; the other one will *expand* to whatever is required to match the aspect ratio of the screen.
- When set to **Shrink**, the *longer* of the width/height of the Canvas's rectangle will be set to be equal to the appropriate parameter; the other one will *shrink* to whatever is required to match the aspect ratio of the screen.
- When set to **MatchWidthOrHeight**, there is an extra parameter, *Match*; setting this to 0 (Width) will make the width of the Canvas match the parameter and stretch or shrink the height to match the screen's aspect ratio; setting it to 1 (Height) will do the opposite; setting it to a value between the two will stretch or shrink both sides to some degree.

## Instantiate issues

There is a small problem with *Reference Resolution*: because there is no method in Unity to *Instantiate* a *GameObject* from code already as a child of another object, new objects must be made first, then their `transform.parent` needs to be set. Unfortunately because of the way *Canvas* objects work, this causes the newly created object to distort, its position, scale or other parameters potentially changing.

The best solution for this issue is to, after setting the `parent`, reset all parameters of the new object to those of its prefab:

```csharp
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
```
    
-----

Created for [Unity 4.6](http://unity3d.com/unity/beta/4.6) beta version 20.

This post is meant for advanced users; for basic information on the new UI features, please check the [offical tutorial videos](http://unity3d.com/learn/tutorials/modules/beginner/ui)!

-----

**Update:** [Unity 4.6](http://unity3d.com/unity/beta/4.6) beta version 21 updated the old *Reference Resolution* component and renamed it **Canvas Scaler**, merging its functionality with *Physical Resolution*, now providing multiple *Scale Mode*s; the functionality described above is still the same when set to *Scale With Screen Size*. The component is now properly documented as well.

The newly added **Reference Pixels Per Unit** parameter is essentially a simple way to scale imported sprites and has an effect on the *Native Size* of Images.
