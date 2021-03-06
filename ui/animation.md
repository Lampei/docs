---
title: Animations
description: Learn how to animate view properties.
position: 9
slug: animations
previous_url: /animation
---

# Animation API

## AnimationDefinition Interface
The [`AnimationDefinition`]({{site.baseurl}}/ApiReference/ui/animation/AnimationDefinition.md) interface is central for defining an animation for **one or more properties** of a **single** [`View`]({{site.baseurl}}/ApiReference/ui/core/view/View.md). The animatable properties are:
 - opacity
 - backgroundColor
 - translateX and translateY
 - scaleX and scaleY
 - rotate

The [`AnimationDefinition`]({{site.baseurl}}/ApiReference/ui/animation/AnimationDefinition.md) interface has the following members:
 - target: The view whose property is to be animated.
 - opacity: Animates the opacity of the view. Value should be a number between 0.0 and 1.0.
 - backgroundColor: Animates the backgroundColor of the view.
 - translate: Animates the translate affine transform of the view. Value should be a [`Pair`]({{site.baseurl}}/ApiReference/ui/animation/Pair.md).
 - scale: Animates the scale affine transform of the view. Value should be a [`Pair`]({{site.baseurl}}/ApiReference/ui/animation/Pair.md).
 - rotate: Animates the rotate affine transform of the view. Value should be a number specifying the rotation amount in degrees.
 - duration: The length of the animation in milliseconds. The default duration is 300 milliseconds.
 - delay: The amount of time, in milliseconds, to delay starting the animation.
 - iterations: Specifies how many times the animation should be played. Default is 1. iOS animations support fractional iterations, i.e. 1.5. To repeat an animation infinitely, use Number.POSITIVE_INFINITY
 - curve: An optional animation curve. Possible values are contained in the [AnimationCurve enumeration]({{site.baseurl}}/ApiReference/ui/enums/AnimationCurve/README.md). Alternatively, you can pass an instance of type [`UIViewAnimationCurve`](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIView_Class/#//apple_ref/c/tdef/UIViewAnimationCurve) for iOS or [`android.animation.TimeInterpolator`](http://developer.android.com/reference/android/animation/TimeInterpolator.html) for Android.

 All members of the interface are **optional** and have default values with the following exceptions:
 - target is only optional when calling the **animate** method of a [`View`]({{site.baseurl}}/ApiReference/ui/core/view/View.md) instance since it is set automatically for you.
 - You must specify at least one property among opacity, backgroundColor, scale, rotate and translate.

## Animation Class
The [`Animation`]({{site.baseurl}}/ApiReference/ui/animation/Animation.md) class represents a **set** of one or more [`AnimationDefinitions`]({{site.baseurl}}/ApiReference/ui/animation/AnimationDefinition.md) which can be played either **simultaneously or sequentially**. **This class is typically used when you need to animate several views together**. The constructor of the the [`Animation`]({{site.baseurl}}/ApiReference/ui/animation/Animation.md) class accepts an array of [`AnimationDefinitions`]({{site.baseurl}}/ApiReference/ui/animation/AnimationDefinition.md) and a boolean parameter indicating whether to play the animations sequentially. Creating an instance of the [`Animation`]({{site.baseurl}}/ApiReference/ui/animation/Animation.md) class does not start the animation playback. The class has four members:
 - play: a method that starts the animation and returns the instance it was called on for fluent animation chaining.
 - cancel: a void method which stops the animation.
 - finished: a promise which will be resolved when the animation finishes or rejected when the animation is cancelled or stops for another reason.
 - isPlaying: a boolean property returning true if the animation is currently playing.

## View.animate Method
In case you need to animate a **single** [`View`]({{site.baseurl}}/ApiReference/ui/core/view/View.md) and you don't need to be able to **cancel** the animation, you can simply use the shortcut **View.animate** method which accepts an [`AnimationDefinition`]({{site.baseurl}}/ApiReference/ui/animation/AnimationDefinition.md), immediately starts the animation and returns its finished promise.

## Animation curves

By default the animation moves with a linear speed without acceleration or deceleration. This might look unnatural and different from the real world where objects need time to reach their top speed and can't stop immediately. The animation curve (called sometimes easing function) is used to give animations an illusion of inertia. It controls the animation speed by modifying the fraction of the duration. NativeScript comes with a number of predefined animation curves:

- The simplest animation curve is linear. It maintains a constant speed while the animation is running:

![linear]({{site.baseurl}}/img/modules/animation/linear.gif "Linear")

- The ease-in curve causes the animation to begin slowly, and then speed up as it progresses. 

![easein]({{site.baseurl}}/img/modules/animation/easein.gif "EaseIn")

- An ease-out curve causes the animation to begin quickly, and then slow down as it completes.

![easeout]({{site.baseurl}}/img/modules/animation/easeout.gif "EaseOut")


- An ease-in ease-out curve causes the animation to begin slowly, accelerate through the middle of its duration, and then slow again before completing.

![easeinout]({{site.baseurl}}/img/modules/animation/easeinout.gif "EaseInOut")


- A spring animation curve causes an animation to produce a spring (bounce) effect.

![spring]({{site.baseurl}}/img/modules/animation/spring.gif "Spring")


In NativeScript the animation curve is represented by the AnimationCurve enumeration and can be specified with the curve property of the animation:

``` JavaScript
view.animate({
	translate: { x: 0, y: 100},    
	duration: 1000,
	curve: enums.AnimationCurve.easeIn
});
```
``` TypeScript
view.animate({
	translate: { x: 0, y: 100},    
	duration: 1000,
	curve: enums.AnimationCurve.easeIn
});
```

It is easy to create your own animation curve by passing in the x and y components of two control points of a cubic Bezier curve. Using Bezier curves is a common technique to create smooth curves in computer graphics and they are widely used in vector-based drawing tools. The values passed to the cubicBezier method control the curve shape. The animation speed will be adjusted based on the resulting path.

![beziergraph]({{site.baseurl}}/img/modules/animation/bezier-graph.png "BezierGraph")

``` JavaScript
view.animate({
    translate: { x: 0, y: 100 },
    duration: 1000,
    curve: enums.AnimationCurve.cubicBezier(0.1, 0.1, 0.1, 1)
});
```
``` TypeScript
view.animate({
    translate: { x: 0, y: 100 },
    duration: 1000,
    curve: enums.AnimationCurve.cubicBezier(0.1, 0.1, 0.1, 1)
});
```

![bezier]({{site.baseurl}}/img/modules/animation/bezier.gif "Bezier")

# Examples

The full source code for all samples is located [`here`](https://github.com/NativeScript/animation-demo).

## Opacity
![opacity]({{site.baseurl}}/img/modules/animation/opacity.gif "Opacity")
``` JavaScript
view.animate({
    opacity: 0,
    duration: 3000
});
```
``` TypeScript
view.animate({
    opacity: 0,
    duration: 3000
});
```

## Background Color
![background-color]({{site.baseurl}}/img/modules/animation/background-color.gif "Background Color")
``` JavaScript
view.animate({
    backgroundColor: new colorModule.Color("#3D5AFE"),
    duration: 3000
});
```
``` TypeScript
view.animate({
    backgroundColor: new colorModule.Color("#3D5AFE"),
    duration: 3000
});
```

## Translate
![translate]({{site.baseurl}}/img/modules/animation/translate.gif "Translate")
``` JavaScript
view.animate({
    translate: { x: 100, y: 100},
    duration: 3000
});
```
``` TypeScript
view.animate({
    translate: { x: 100, y: 100},
    duration: 3000
});
```

## Scale
![scale]({{site.baseurl}}/img/modules/animation/scale.gif "Scale")
``` JavaScript
view.animate({
    scale: { x: 2, y: 2},
    duration: 3000
});
```
``` TypeScript
view.animate({
    scale: { x: 2, y: 2},
    duration: 3000
});
```

## Rotate
![rotate]({{site.baseurl}}/img/modules/animation/rotate.gif "Rotate")
``` JavaScript
view.animate({
    rotate: 360,
    duration: 3000
});
```
``` TypeScript
view.animate({
    rotate: 360,
    duration: 3000
});
```

## Multiple Properties
![multiple-properties]({{site.baseurl}}/img/modules/animation/multiple-properties.gif "Multiple Properties")
``` JavaScript
view.animate({
    backgroundColor: new color.Color("#3D5AFE"),
    opacity: 0.5,
    translate: { x: 100, y: 100 },
    rotate: 180,
    duration: 3000
});
```
``` TypeScript
view.animate({
    backgroundColor: new color.Color("#3D5AFE"),
    opacity: 0.5,
    translate: {x: 100, y: 100},
    rotate: 180,
    duration: 3000
});
```

## Chaining with Promises
![chaining-with-promises]({{site.baseurl}}/img/modules/animation/chaining-with-promises.gif "Chaining with Promises")
``` JavaScript
view.animate({ opacity: 0 })
    .then(function () { return view.animate({ opacity: 1 }); })
    .then(function () { return view.animate({ translate: { x: 100, y: 100 } }); })
    .then(function () { return view.animate({ translate: { x: 0, y: 0 } }); })
    .then(function () { return view.animate({ scale: { x: 3, y: 3 } }); })
    .then(function () { return view.animate({ scale: { x: 1, y: 1 } }); })
    .then(function () { return view.animate({ rotate: 180 }); })
    .then(function () { return view.animate({ rotate: 0 }); })
    .then(function () {
    console.log("Animation finished");
})
    .catch(function (e) {
    console.log(e.message);
});
```
``` TypeScript
view.animate({ opacity: 0 })
    .then(() => view.animate({ opacity: 1 }))
    .then(() => view.animate({ translate: { x: 100, y: 100 } }))
    .then(() => view.animate({ translate: { x: 0, y: 0 } }))
    .then(() => view.animate({ scale: { x: 3, y: 3 } }))
    .then(() => view.animate({ scale: { x: 1, y: 1 } }))
    .then(() => view.animate({ rotate: 180 }))
    .then(() => view.animate({ rotate: 0 }))
    .then(() => {
    console.log("Animation finished");
  })
    .catch((e) => {
    console.log(e.message);
  });
```

## Chaining with Animation Set
![chaining-with-animation-set]({{site.baseurl}}/img/modules/animation/chaining-with-animation-set.gif "Chaining with Animation Set")
``` JavaScript
var definitions = new Array();
definitions.push({ target: view1, translate: { x: 200, y: 0 }, duration: 3000 });
definitions.push({ target: view2, translate: { x: 0, y: 200 }, duration: 3000 });
definitions.push({ target: view3, translate: { x: -200, y: 0 }, duration: 3000 });
definitions.push({ target: view4, translate: { x: 0, y: -200 }, duration: 3000 });
var playSequentially = true;
var animationSet = new animationModule.Animation(definitions, playSequentially);
animationSet.play().then(function () {
    console.log("Animation finished");
})
    .catch(function (e) {
    console.log(e.message);
});
```
``` TypeScript
var definitions = new Array<animationModule.AnimationDefinition>();
definitions.push({target: view1, translate: {x: 200, y: 0}, duration: 3000 });
definitions.push({target: view2, translate: {x: 0, y: 200}, duration: 3000 });
definitions.push({target: view3, translate: {x: -200, y: 0}, duration: 3000 });
definitions.push({target: view4, translate: {x: 0, y: -200}, duration: 3000 });
var playSequentially = true;
var animationSet = new animationModule.Animation(definitions, playSequentially);
animationSet.play().then(() => {
    console.log("Animation finished");
})
.catch((e) => {
    console.log(e.message);
});
```

## Multiple Views
![multiple-views]({{site.baseurl}}/img/modules/animation/multiple-views.gif "Multiple Views")
``` JavaScript
var definitions = new Array();
var a1 = {
    target: view1,
    translate: { x: 200, y: 0 },
    duration: 3000
};
definitions.push(a1);
var a2 = {
    target: view2,
    translate: { x: 0, y: 200 },
    duration: 3000
};
definitions.push(a2);
var a3 = {
    target: view3,
    translate: { x: -200, y: 0 },
    duration: 3000
};
definitions.push(a3);
var a4 = {
    target: view4,
    translate: { x: 0, y: -200 },
    duration: 3000
};
definitions.push(a4);
var animationSet = new animationModule.Animation(definitions);
animationSet.play().then(function () {
    console.log("Animation finished");
})
    .catch(function (e) {
    console.log(e.message);
});
```
``` TypeScript
var definitions = new Array<animationModule.AnimationDefinition>();
var a1: animationModule.AnimationDefinition = {
    target: view1,
    translate: {x: 200, y: 0},
    duration: 3000
};
definitions.push(a1);

var a2: animationModule.AnimationDefinition = {
    target: view2,
    translate: {x: 0, y: 200},
    duration: 3000
};
definitions.push(a2);

var a3: animationModule.AnimationDefinition = {
    target: view3,
    translate: {x: -200, y: 0},
    duration: 3000
};
definitions.push(a3);

var a4: animationModule.AnimationDefinition = {
    target: view4,
    translate: {x: 0, y: -200},
    duration: 3000
};
definitions.push(a4);

var animationSet = new animationModule.Animation(definitions);

animationSet.play().then(() => {
    console.log("Animation finished");
})
.catch((e) => {
    console.log(e.message);
});
```

## Reusing Animations
![reusing]({{site.baseurl}}/img/modules/animation/reusing.gif "Reusing Animations")
``` JavaScript
var animation1 = view.createAnimation({ opacity: 0 });
var animation2 = view.createAnimation({ opacity: 1 });
animation1.play()
    .then(function () { return animation2.play(); })
    .then(function () { return animation1.play(); })
    .then(function () { return animation2.play(); })
    .then(function () { return animation1.play(); })
    .then(function () { return animation2.play(); })
    .then(function () {
    console.log("Animation finished");
})
    .catch(function (e) {
    console.log(e.message);
});
```
``` TypeScript
var animation1 = view.createAnimation({opacity: 0});
var animation2 = view.createAnimation({opacity: 1});

animation1.play()
.then(()=>animation2.play())
.then(()=>animation1.play())
.then(()=>animation2.play())
.then(()=>animation1.play())
.then(()=>animation2.play())
.then(() => {
    console.log("Animation finished");
})
.catch((e) => {
    console.log(e.message);
});
```

## Slide-in Effect
![slide-in-effect]({{site.baseurl}}/img/modules/animation/slide-in-effect.gif "Slide-in Effect")
``` JavaScript
var item = new imageModule.Image();
item.src = "~/res/icon_100x100.png";
item.width = 90;
item.height = 90;
item.style.margin = "5,5,5,5";
item.translateX = -300;
item.opacity = 0;
item.on("loaded", function (args) {
    args.object.animate({ translate: { x: 0, y: 0 }, opacity: 1 });
});
wrapLayout.addChild(item);
```
``` TypeScript
var item = new imageModule.Image();
item.src = "~/res/icon_100x100.png";
item.width = 90;
item.height = 90;
item.style.margin = "5,5,5,5";
item.translateX = -300;
item.opacity = 0;
item.on("loaded", (args: observable.EventData) => {
    (<viewModule.View>args.object).animate({translate: { x: 0, y: 0 }, opacity: 1});
});
wrapLayout.addChild(item);
```

## Infinite
![infinite]({{site.baseurl}}/img/modules/animation/infinite.gif "Infinite")
``` JavaScript
animationSet = new animationModule.Animation([{
        target: view,
        rotate: 360,
        duration: 3000,
        iterations: Number.POSITIVE_INFINITY,
        curve: view.ios ? UIViewAnimationCurve.UIViewAnimationCurveLinear : new android.view.animation.LinearInterpolator
    }]);
animationSet.play().catch(function (e) {
    console.log("Animation stoppped!");
});
// Call animationSet.cancel() to stop it;
```
``` TypeScript
animationSet = new animationModule.Animation([{
    target: view,
    rotate: 360,
    duration: 3000,
    iterations: Number.POSITIVE_INFINITY,
    curve: view.ios ? UIViewAnimationCurve.UIViewAnimationCurveLinear : new android.view.animation.LinearInterpolator
}]);
animationSet.play().catch((e) => {
    console.log("Animation stoppped!");
});
// Call animationSet.cancel() to stop it;
```

