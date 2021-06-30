## Keyboard Animation

~~In the pass, if you want to move your view alongside with keyboard show/hide animation, 
you need to read other infomation included in the notification like duration and curve 
to generate the animation yourself but I've just realized that now you only need to update
the view's frame, constraints, etc... and the system will animate that chnage for free.~~

I was totally wrong. This is still neccessary.

```Swift
public enum Utils {
  /// Animate changes to one or more views using the same option and duraion
  /// as the keyboard animation which is fetched from a notification sent by the system.
  /// - Parameters:
  ///   - notification: The notificaion object recieved from the system.
  ///   - animations: A block object containing the changes to commit to the views.
  ///   The keyboard frame is also passed to this block.
  ///
  /// The `animations` block will be called with `CGRect.zero`
  /// when any of necessary infomation is not found in the notification.
  static public func keyboardAnimation(
    from notification: Notification,
    animations: @escaping (CGRect) -> Void,
    completion: ((Bool) -> Void)? = { _ in }
  ) {
    let frameKey = UIResponder.keyboardFrameEndUserInfoKey
    let durationKey = UIResponder.keyboardAnimationDurationUserInfoKey
    let curveKey = UIResponder.keyboardAnimationCurveUserInfoKey

    guard
      let frame = (notification.userInfo?[frameKey] as? NSValue)?.cgRectValue,
      let duration = notification.userInfo?[durationKey] as? Double,
      let curve = notification.userInfo?[curveKey] as? UInt
    else {
      animations(.zero)
      return
    }

    UIView.animate(
      withDuration: duration,
      delay: 0,
      options: UIView.AnimationOptions(rawValue: curve),
      animations: { animations(frame) },
      completion: completion)
  }
}
```
