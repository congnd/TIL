## Keyboard Animation

In the pass, if you want to move your view alongside with keyboard show/hide animation, 
you need to read other infomation included in the notification like duration and curve 
to generate the animation yourself but I've just realized that now you only need to update
the view's frame, constraints, etc... and the system will animate that chnage for free.

Past
```Swift
func handle(notification: Notification) {
  let frameKey = UIResponder.keyboardFrameEndUserInfoKey
  let durationKey = UIResponder.keyboardAnimationDurationUserInfoKey
  let curveKey = UIResponder.keyboardAnimationCurveUserInfoKey

  guard
    let frame = (notification.userInfo?[frameKey] as? NSValue)?.cgRectValue,
    let duration = notification.userInfo?[durationKey] as? Double,
    let curve = notification.userInfo?[curveKey] as? UInt
  else { return }

  UIView.animate(
    withDuration: duration,
    delay: 0,
    options: UIView.AnimationOptions(rawValue: curve),
    animations: { 
      // Updage layout here
    })
}
```

Now
```Swift
func handle(notification: Notification) {
  let frameKey = UIResponder.keyboardFrameEndUserInfoKey
  guard let frame = (notification.userInfo?[frameKey] as? NSValue)?.cgRectValue else { return }
  
  // update layout here
}
```
