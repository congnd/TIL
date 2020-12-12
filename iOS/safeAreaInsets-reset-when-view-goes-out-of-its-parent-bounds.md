## `safeAreaInsets` is reset when the view goes out of its parent's bounds

Most of the case your views are always fit within the bounds of its parent,
but in some special cases, you may want to temporarily set the view's frame a bit bigger than it's neeed.
And in those cases, you view's frame can goes out of the bounds of its parent.

For example, when animate a view or change a view's position, you may want to use `UIView.animate()` or change the frame directly.
Whenever your view's frame goes out of it's parent, it's `safeAreaInsets` will be set to `UIEdgeInsets.zero`.
And this can cause your layout changes unexpectedly. 

If this is your expected behavior (most of the cases), then you don't need to do anything. 
But for example, if you are using damping animation when presenting a view controller modally where your view is expected to be fluctuated a bit,
and you don't want the bottom of the screen can be seen through your presented view controller while fluctuating.
Then you may want to give your view a hight value which is a bit greater than the ideal value while animateing.
After the animation you are going to set the view's frame back to the ideal value which is within the parenet's bounds.
In this case, the `safeAreaInsets` of your view might be changed unexpectedly while the aniamtion is in progress.

## `CAAnimation` to resecue

You can use `CAAnimation` to animate views without affecting the `safeAreaInsets`.
When using `CAAniamtion` we are not going to touch to the `frame` of the view, but we control the view via some animatable properties of its layer.
Therefore, there is no frame changes and of course, it will not cause any affect to the `safeAreaInsets`

This is my customized presentation controller which allows panning the presented view controller with the damping animation.
You may need to tweak the animatino configuration a bit to get the result you want.
```Swift
class ModalPresentationController: UIPresentationController {
  var maxRect: CGRect {
    let top = containerView!.safeAreaInsets.top + 60
    let origin = CGPoint(x: 0, y: top)
    let containerSize = containerView!.bounds.size
    let size = CGSize(width: containerSize.width, height: containerSize.height - top)
    return CGRect(origin: origin, size: size)
  }

  var minRect: CGRect {
    let top = containerView!.bounds.height / 2
    let origin = CGPoint(x: 0, y: top)
    let containerSize = containerView!.bounds.size
    let size = CGSize(width: containerSize.width, height: containerSize.height - top)
    return CGRect(origin: origin, size: size)
  }

  override init(
    presentedViewController: UIViewController,
    presenting presentingViewController: UIViewController?
  ) {
    super.init(
      presentedViewController: presentedViewController,
      presenting: presentingViewController)

    let panGestureRecognizer = UIPanGestureRecognizer()
    panGestureRecognizer.addTarget(self, action: #selector(onPan(pan:)))
    presentedViewController.view.addGestureRecognizer(panGestureRecognizer)
  }

  var prevY: CGFloat = 0

  @objc func onPan(pan: UIPanGestureRecognizer) {
    guard let containerView = containerView, let presentedView = presentedView else { return }

    let transitionY = pan.translation(in: pan.view?.superview).y
    let targetY = max(prevY + transitionY, maxRect.origin.y)

    switch pan.state {
    case .began:
      prevY = presentedView.frame.origin.y

    case .changed:
      presentedView.frame.origin.y = targetY
      presentedView.frame.size.height = containerView.bounds.height - targetY

    case .ended, .cancelled:
      let velocityY = pan.velocity(in: containerView).y
      finishAnimation(velocityY: velocityY, targetY: targetY)

    case .possible, .failed:
      break

    @unknown default:
      break
    }
  }

  func finishAnimation(velocityY: CGFloat, targetY: CGFloat) {
    let acceletatedTargetY = targetY + velocityY

    if acceletatedTargetY > containerView!.bounds.height * 0.6 {
      presentedViewController.dismiss(animated: true)
      return
    }

    let extraY: CGFloat = 60

    let posY = presentedView!.frame.minY
    let safeFrame = CGRect(
      x: maxRect.minX,
      y: maxRect.minY - extraY,
      width: maxRect.width,
      height: maxRect.height + extraY)
    presentedView!.frame = safeFrame

    let startY = posY + safeFrame.height / 2
    let endY = maxRect.midY + extraY / 2
    let initialVelocityY = velocityY / (-containerView!.bounds.height / 4)

    let animation = CASpringAnimation(keyPath: #keyPath(CALayer.position))
    animation.damping = 20
    animation.mass = 1
    animation.stiffness = 200
    animation.initialVelocity = initialVelocityY
    animation.fromValue = CGPoint(x: safeFrame.midX, y: startY)
    animation.toValue = CGPoint(x: safeFrame.midX, y: endY)
    animation.fillMode = CAMediaTimingFillMode.forwards
    animation.isRemovedOnCompletion = false
    animation.timingFunction = CAMediaTimingFunction(name: .linear)

    animation.duration = animation.settlingDuration

    let animationKey = "damping-\(UUID().hashValue)"
    CATransaction.begin()
    CATransaction.setCompletionBlock({
      self.presentedView!.layer.removeAnimation(forKey: animationKey)
      self.presentedView!.frame = self.maxRect
    })
    presentedView!.layer.add(animation, forKey: animationKey)
    CATransaction.commit()
  }

  override var frameOfPresentedViewInContainerView: CGRect {
    minRect
  }
}

```
