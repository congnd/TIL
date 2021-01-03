## Having fun with CAAnimation

Let's try to create an animation like this using pure CAAnimation.

![](https://media.giphy.com/media/nVD6nYqEohCsD25P3o/giphy.gif)

This animation is constructed by combining the following pieces:
- rocket animation: pushes the ballons up at a fast speed to simulate a rocket's taking off
- spring animation: rotates and scales the ballons to make it look like being pressed in a strong pressure
- bezier animation: moves the ballons up along a bezier path to make it look like being moved naturally
- 3d rotation animation: rotates the ballons 3D to make it look like being affected by wind
- opacity animation: goes away smoothly

We'll go through all of them in turn.

### Rocket animation

This animation mimics the rocket's taking off effect in a small portion of time. 
It does nothing but moving the layer from the start point to the end point along a straight line.

### Spring animation

This animation occurs simutaniously with the rocket animation above to make the animated object
more interesting by adding a bouncing animation. So basically, we are not going to move the layer
in this animation but just make it bigger and then smaller overtime. 

To make the layer look like a rocket, we scale horizontally the layer to a very small value 
in the begining and then scale it back to a normal aspect in the end of the animation.

After combining this with the rocket animation above, our animation now looks like this:

![](https://media.giphy.com/media/avpqHEufHTlNZ0PJCr/giphy.gif)

### Bezier animation
After reaching the endpoint of the rocket animation, we will move the object along a bezier path. 
If you don't know what a bezier path is then you should take a look at [this link](https://en.wikipedia.org/wiki/B%C3%A9zier_curve).

In short, bezier paths are curves which are defined by a set of `control points` to create 
a smooth connection between the start and the end point. I'm not going to deep dive into 
the detail of Bezier paths because it's quite complicated and painful to explain 
so if you are interested, please read the link above. But you can look at the below images for easy to iamage what are they.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/3d/B%C3%A9zier_2_big.gif/240px-B%C3%A9zier_2_big.gif" width="240"/>
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a4/B%C3%A9zier_4_big.gif/240px-B%C3%A9zier_4_big.gif" width="240"/>

This time, we will use bezier path animation to make the view less boring and more natural.

Create bezier animation is quite easy. All we have to do is create a bezier path then CAAnimation will handle the rest for us.

Create Bezier path.
```Swift
let path = UIBezierPath()
path.move(to: startPoint)

let endPoint = CGPoint(x: posX + CGFloat.random(in: ), y: 0)
let controlPoint1 = CGPoint(x: , y: )
let controlPoint2 = CGPoint(x: , y: )
path.addCurve(to: endPoint, controlPoint1: controlPoint1, controlPoint2: controlPoint2)
```

Then create CAAnimation like this:
```Swift
let animation = CAKeyframeAnimation(keyPath: #keyPath(CALayer.position))
animation.path = path.cgPath
```

With this setup, your bezier animation looks like this:

![](https://media.giphy.com/media/0BvUKguh3rL7FNQQ2m/giphy.gif)

### 3D rotation animation
We can easily create this effect by animating the `transform` property on CALayer 
which is also an animatable property. Since CAAnimation supports rotations over 
all 3 axises but there is no depth of field effect so we will add scale factors 
to somehow simulate that effect to make it more like real 3D rotation.

Here is the sample code:
```Swift
let animation = CABasicAnimation(keyPath: #keyPath(CALayer.transform))

let angle = CGFloat.random(in: ) * CGFloat.pi
let rotation = CATransform3DMakeRotation(angle, 0, 1, 0)
let finalScale = CGFloat.random(in: 2...5)
let transoform = CATransform3DScale(rotation, finalScale, finalScale, 1)
animation.toValue = transoform
```

With this animation only, it looks like this:

![](https://media.giphy.com/media/ypwEVIY4yQwtHsDzEp/giphy.gif)


### Opacity animation
Probably, this is the easiest one. Just use animate a basic animation which controls the opacity value of the layer.

```Swift
let animation = CABasicAnimation(keyPath: #keyPath(CALayer.opacity))
animation.toValue = 0
```


### Combining all the animations together

CAAnimation allows us to group multiple animations into one and run them concurently. 
We'll do it by using a subclass of CAAnimation called CAAnimationGroup. 
Configuring CAAnimationGroup is not that hard except one thing that we have to configure 
the begin time and end time of all the sub animations properly in order to achive what we want.

```Swift
let animationGroup = CAAnimationGroup()
animationGroup.animations = [
  // put your sub animations here
]
```

Then control the whole animation's duration as usual:

```Swift
animationGroup.duration = duration
```

You probably want to do something like cleaning after the animation completed. If so, you can use CATransaction:

```Swift
CATransaction.begin()
CATransaction.setCompletionBlock {
  // perform cleaning here
}
iconView.layer.add(animationGroup, forKey: nil)
CATransaction.commit()
```

After combining all the animations above together, eventually we'll get this final animation:

![](https://media.giphy.com/media/nVD6nYqEohCsD25P3o/giphy.gif)

Checkout the source code here: [https://github.com/congnd/FunWithCoreAnimation](https://github.com/congnd/FunWithCoreAnimation)
