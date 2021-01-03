## Strange behavior when animating CATranform3D

I've found the issue when trying to fulfill the following requirement:
Implement an animation which animates a 45 degree rotated view from 0-scaled X-axis to 0.2-scaled X-axis.

The code seems to be very simple like this:
```Swift
let r = CATransform3DMakeRotation(.pi / 4, 0, 0, 1)

let s1 = CATransform3DMakeScale(0, 1, 1)
let s2 = CATransform3DMakeScale(0.2, 1, 1)

let a = CABasicAnimation(keyPath: #keyPath(CALayer.transform))

a.fromValue = CATransform3DConcat(s1, r)
a.toValue = CATransform3DConcat(s2, r)
a.duration = 2

view.layer.add(a, forKey: nil)
```

And you may expect something like this:

![](https://media.giphy.com/media/veDtGxby1ak4q02p3y/giphy.gif)

But eventually, you will see something like this:

![](https://media.giphy.com/media/1M5VXOMMtg7U1bHvpc/giphy.gif)

It's really weird, right?

To be honest, I really don't understand why this can happen.
But I realized that if we scale the view by a value which closes to zero but not `zero` (for example 0.0001) then the issue goes away.
And you'll see what you've expected.

Instead of
```Swift
let s1 = CATransform3DMakeScale(0, 1, 1)
```

Use this:
```Swift
let s1 = CATransform3DMakeScale(0.0001, 1, 1)
```
