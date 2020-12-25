## Interactable Presentation

UIKit brings many transition animations such as push, pop, cover vertically, 
interactable modal,... and many more. Most of the cases, you will don't need
to create your own ones, just go ahead with the system provided one is enough. 
But if you want to do it to improve your user's experiences and make your app exclusive,
you can do it with many options that the system has already provided. It's a bit lengthy
when compared to the default one of course, but you will fond that it's not as tough as you may think.

This article helps you to make this kind of customized presentation.

<img src="https://media.giphy.com/media/RHoaaVhDUUbqsCI0gk/giphy.gif" width="240"/>

### UIKit's presentation components

<img src="https://i.postimg.cc/vmxgGmfj/custom-presentation.jpg" width="740"/>

When a view controller is presented, it asks for controller objects that manage 
all the aspects of the transition including: animation, user interaction and presentation.

A full transition can be controlled by the following components:
- PresentationController: objects that describe what the presneted view controller 
  looks like after the transition. It can decorate the presented view controller as well.
- AnimatedTrasitioning: animators object that add animations for the transition.
- InteractiveTransitioning: controls the animated trasition by user interaction.
  For example, we can slow down, speed up or revert the animation based on user interactions.
  This object does only one thing - control the animation. It does NOT create any animation.
  Therefore, you must provide the animated transitioning object to make the interactive transition works

Now you know all the basic about the transition process on iOS. Let's try to make a custom one.

### Custom transition
As you can can see in the image on top of this article, the presented view controller
is presented as a side menu (drawer). It will be translated from the left edge to 
~3/4 width of the screen, and we can swipe left/right to open/close the side menu.

Let's deep dive into the implementation!

#### Set transition delegate and presentation style
To let the system know that you want to use a custom transition but not the default animation,
you need to set the `modalPresentationStyle` and the `transitioningDelegate` for the presented view controller.

```Swift
vc.modalPresentationStyle = .custom
vc.transitioningDelegate = transitioningCoordinator
```

***The detail of the `transitioningCoordinator` will be explained below***

#### Create a customized presentation controller

I will create a custom presentation controller which adds a dimming view 
underneath the presented view controller. And I'll animate the alpha value 
of the dimming view along side with animations created by the animators.

You can add animations on top of the animations created by animators 
but I'd not recommed creating animation within the presentation object.

We will put the side menu spreading 3/4 width of the screen from the left side.
We do it by overriding the `frameOfPresentedViewInContainerView` and 
return a proper CGRect value.

Our presentation looks like this:

```Swift
class PresentationController: UIPresentationController {
  var dimmingView: UIView?

  override var frameOfPresentedViewInContainerView: CGRect {
    guard let containerView = containerView else { return .zero }
    let size = CGSize(width: 320, height: containerView.bounds.height)
    return CGRect(origin: .zero, size: size)
  }

  override func presentationTransitionWillBegin() {
    if dimmingView == nil {
      let dimmingView = UIView()
      dimmingView.frame = containerView!.bounds
      dimmingView.backgroundColor = .black
      dimmingView.alpha = 0
      self.dimmingView = dimmingView
      containerView!.addSubview(dimmingView)
    }

    let transitionCoordinator = presentingViewController.transitionCoordinator!
    transitionCoordinator.animate(alongsideTransition: { _ in
      self.dimmingView?.alpha = 0.5
    })
  }

  override func dismissalTransitionWillBegin() {
    let transitionCoordinator = presentingViewController.transitionCoordinator!
    transitionCoordinator.animate(alongsideTransition: { _ in
      self.dimmingView?.alpha = 0
    })
  }
}
```

#### Create animators

Our animator object is in charge of moving the presented view controller from left edge side 
to the ~3/4 width of the screen. To simplify the implementation, we will use only 1 animator 
for both presentation and dismissal. 

The animation here is preaty simple. Just a few lines of code as below.
Be aware that when presenting, the presented view controller can be fetched via the 
`UITransitionContextViewControllerKey.to)` and `UITransitionContextViewControllerKey.from)` 
for the presenting view controller. But when disimissing, the key will will be reversed and 
`.to` for the presenting and the `.from` for the presented view controller.

The duration value returned in the `transitionDuration(using:)` must be equal 
with the actual animation duration.

We are responsibe for adding the view controller's view into the `containerView`.

When the animation completed, we have to notify the system about that by calling the 
`transitionContext.completeTransition()`. In case the transition has been cancelled by 
user interaction, the system will set the `transitionWasCancelled` on the `transitionContext` to true.
Therefore, you must pass the `!transitionContext.transitionWasCancelled` to the completion 
method to prevent unexpected behaviors.

```Swift
class AnimatedTransitioning: NSObject, UIViewControllerAnimatedTransitioning {
  let isPresenting: Bool

  init(isPresenting: Bool) {
    self.isPresenting = isPresenting
    super.init()
  }

  func transitionDuration(using transitionContext: UIViewControllerContextTransitioning?) -> TimeInterval {
    return 0.3
  }

  func animateTransition(using transitionContext: UIViewControllerContextTransitioning) {
    if isPresenting {
      let presented = transitionContext.viewController(forKey: .to)!
      let presenting = transitionContext.viewController(forKey: .from)!
      let containerView = transitionContext.containerView

      containerView.addSubview(presented.view)
      presented.view.frame = transitionContext.viewController(forKey: .to)!.presentationController!.frameOfPresentedViewInContainerView
      presented.view.frame.origin.x = -presented.view.frame.width
      UIView.animate(withDuration: 0.3, animations: {
        presented.view.frame.origin.x = 0
        presenting.view.frame.origin.x = presented.view.frame.width
      }) { _ in
        transitionContext.completeTransition(!transitionContext.transitionWasCancelled)
      }
    } else {
      let presented = transitionContext.viewController(forKey: .from)!
      let presenting = transitionContext.viewController(forKey: .to)!

      UIView.animate(withDuration: 0.3, animations: {
        presented.view.frame.origin.x = -presented.view.frame.width
        presenting.view.frame.origin.x = 0
      }) { _ in
        transitionContext.completeTransition(!transitionContext.transitionWasCancelled)
      }
    }
  }
}
```

#### Response to transitioning delegate
Now we already have the customized presentation and the animation, let provide them to the system 
by implementing the `UIViewControllerTransitioningDelegate`.

You can make any existed class conform to the protocol, but in this case I'll create a new one.

```Swift
class TransistioningCoordinator: NSObject, UIViewControllerTransitioningDelegate {
  var presentationController: PresentationController!

  func animationController(forPresented presented: UIViewController, presenting: UIViewController, source: UIViewController) -> UIViewControllerAnimatedTransitioning? {
    return AnimatedTransitioning(isPresenting: true)
  }

  func animationController(forDismissed dismissed: UIViewController) -> UIViewControllerAnimatedTransitioning? {
    return AnimatedTransitioning(isPresenting: false)
  }

  func presentationController(forPresented presented: UIViewController, presenting: UIViewController?, source: UIViewController) -> UIPresentationController? {
    let presentation = PresentationController(presentedViewController: presented, presenting: presenting)
    presentationController = presentation
    return presentation
  }
}
```

Now, every time we call the `present()`, the system will use your provided presentation object and animators
to drive the transition. And it looks exactly like the image above but without nay interaction support.

Next, let's move on adding support interactive transitioning.

### Adding interactive transitioning

#### Making the interaction controller

Our custom transition support swipe gesture for both opening and closing transition.
For the opening transtion, we need to setup the gesture on the presenting view controller.
And for the closing transition, we need to setup an other gesture on the presented view controller
or on the presentation controller. In this implementation, I'm going to put 
the gesture recognizer to the presenting view controller for openning transition and 
an other gesture recognizer to the presentation controller's content view for closing transition.

Since most of the logic for that 2 gestures are the same, I'll create only one class to handle them.

`SwipeInteractionController` receives a view on which the gesture recognizer will be inserted to.

```Swift
class SwipeInteractionController {
  /// Indicates that the interaction is in progress
  var isInteracting: Bool = false
  
  /// Pan starts
  var begin: () -> () = {}
  
  /// Pan updated. The value passed to this closure is the moved distance of the pan.
  var update: (CGFloat) -> () = { _ in }
  
  /// Pan cancelled or finished.
  var end: (CGFloat) -> () = { _ in }

  init(view: UIView) {
    prepareGestureRecognizer(in: view)
  }

  private func prepareGestureRecognizer(in view: UIView) {
    let gesture = UIPanGestureRecognizer(target: self, action: #selector(handleGesture(_:)))
    view.addGestureRecognizer(gesture)
  }

  @objc func handleGesture(_ gestureRecognizer: UIScreenEdgePanGestureRecognizer) {
    let translation = gestureRecognizer.translation(in: nil).x

    switch gestureRecognizer.state {
    case .began:
      isInteracting = true
      begin()
    case .changed:
      update(translation)
    case .cancelled, .ended:
      isInteracting = false
      end(translation)
    default:
      break
    }
  }
}
```

#### Installing the opening interaction controller

Now, we are going to install the interaction controller created above into 
the presenting view controller via the `TransistioningCoordinator`.

Update the TransistioningCoordinator like this:

```Swift
class TransistioningCoordinator: NSObject, UIViewControllerTransitioningDelegate {
  let interactionController: SwipeInteractionController
  // ...
  
  init(view: UIView) {
    interactionController = .init(view: view)
  }
}
```

And then, update the presenting view controller:

```Swift

class ViewController: UIViewController {
  lazy var transitioningCoordinator: TransistioningCoordinator = {
    return TransistioningCoordinator(view: view)
  }()
  // ...
}
```

#### Installing the closing interaction controller

Update the `PresentationController` like below:

```Swift
class PresentationController: UIPresentationController {
  // ...
  override func presentationTransitionDidEnd(_ completed: Bool) {
    if completed {
      let interactionController = SwipeInteractionController(view: containerView!)
      self.interactionController = interactionController
      interactionController.begin = { [unowned self] in
        self.presentingViewController.dismiss(animated: true)
      }
    }
  }
  // ...
}
```

The presentation controller is in charge of triggering the dismissal transition when 
receiving `begin` event from the interaction controller indicates that user interaction did begin.

#### Creating proper interactive transitioning objects

At this step, we have almost everything we need: the animators, the presentation objects and
the interactions handlers. We know that we have to create the interactive transitioning, 
but how does the interactive transitioning drive the animation? UIKit provides a very handful 
concrete implementation of the `UIViewControllerInteractiveTransitioning` called 
`UIPercentDrivenInteractiveTransition`. Use can use this class to control how much of 
the transition is completed depends on the current interaction progress.

In this case, we will use the moved distance of the pan gesture to control the progress of the transition animation.

Add the following into the `TransistioningCoordinator`:

```Swift
class TransistioningCoordinator: NSObject, UIViewControllerTransitioningDelegate {
func interactionControllerForDismissal(using animator: UIViewControllerAnimatedTransitioning) -> UIViewControllerInteractiveTransitioning? {
    guard let interactionController = dimissalInteractionController else { return nil }

    // If the trasition is triggered by code, then you may not want the interactive transition evolves
    if interactionController.isInteracting {
      let transition = UIPercentDrivenInteractiveTransition()
      
      // Calculate how much the transition is completed.
      func toPercent(_ moved: CGFloat) -> CGFloat {
        let width = presentationController.frameOfPresentedViewInContainerView.width
        return -(moved / width)
      }
      
      // Update the transition.
      interactionController.update = { moved in
        transition.update(toPercent(moved))
      }
      
      // Deciding whether the transition should be finished or cancelled bases on the moved distance.
      // We can add the current velocity as an input factor here to increate the naturality of the transition.
      interactionController.end = { moved in
        if toPercent(moved) > 0.5 {
          transition.finish()
        } else {
          transition.cancel()
        }
      }
      return transition
    }

    return nil
  }

  func interactionControllerForPresentation(using animator: UIViewControllerAnimatedTransitioning) -> UIViewControllerInteractiveTransitioning? {
    if interactionController.isInteracting {
      let transition = UIPercentDrivenInteractiveTransition()
      
      // Notice that this function is different from the above one.
      func toPercent(_ moved: CGFloat) -> CGFloat {
        let width = presentationController.frameOfPresentedViewInContainerView.width
        return moved / width
      }
      
      interactionController.update = { moved in
        transition.update(toPercent(moved))
      }
      
      interactionController.end = { moved in
        if toPercent(moved) > 0.5 {
          transition.finish()
        } else {
          transition.cancel()
        }
      }
      return transition
    }

    return nil
  }
}
```

Okay, now just build and run your app to enjoy the interactable transition.

You can find the full implementation here:
[https://github.com/congnd/InteractivePresentation](https://github.com/congnd/InteractivePresentation)

