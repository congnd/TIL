## Add spacing between image and title of UIButton

It's very common to us - developers to add spacing between title and image of an UIButton.
Although UIButton doesn't provide us a direct way to do so, but we can achieve the same result
by rely on the `edagesInsets`s.

```Swift
extension UIButton {
  /// Set spacing between image and title of the button
  /// - Parameter spacing: the desired spacing
  func set(spacing: CGFloat) {
    let amount = spacing / 2
    imageEdgeInsets = UIEdgeInsets(top: 0, left: -amount, bottom: 0, right: amount)
    titleEdgeInsets = UIEdgeInsets(top: 0, left: amount, bottom: 0, right: -amount)
    contentEdgeInsets = UIEdgeInsets(top: 0, left: amount, bottom: 0, right: amount)
  }
}
```
