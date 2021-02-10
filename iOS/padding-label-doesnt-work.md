## PaddingLabel doesn't really work

Many people out there are trying to subclass UILabel to adding padding ability to the label.
A common implementation may look like this:

```Swift
class PaddingLabel: UILabel {
  private let insets: UIEdgeInsets

  init(insets: UIEdgeInsets) {
    self.insets = insets
    super.init(frame: CGRect.zero)
  }

  override func drawText(in rect: CGRect) {
    super.drawText(in: rect.inset(by: insets))
  }

  override var intrinsicContentSize: CGSize {
    var size = super.intrinsicContentSize
    size.width += insets.left + insets.right
    size.height += insets.top + insets.bottom
    return size
  }
}
```

But I've just realized that, this implementation may work fine with system fonts 
but it desn't work quite well with custom fonts.

So the solution is, just use the UILabel and add it to a UIView as container. Then the rest is up to you.
