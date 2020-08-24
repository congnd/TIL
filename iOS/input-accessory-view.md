### Making any custom view sticks on the top the keyboard

iOS already provides a capability that allows us to add a custom view on top of the keyboard.

`UIResponder` has a property called `inputAccessoryView`, we can override this property 
on `UIResponder` subclasses (UIViewController and UIView are subclasses of `UIResponder`) 
and then provide our own custom view that will be displayed on top of the keyboard.
When the keyboard is not displayed but the `UIResponder` object is still the first responder,
the system will stick our custom view to the bottom of the screen. 

Let say we have a view controller called `ChatViewController`. 
When this view controller is displayed, we want to show an input view in the bottom of the screen.
When the user taps into the text view inside the input view, we will show the keyboard
but keep the input sticks on the top of the keyboard.

```Swift 
class InputView: UIView {
  // Without this, your custom view will not be displayed on the screen and you will be a bunch of constraint warnings.
  override var intrinsicContentSize: CGSize { .zero }
}
```

```Swift
class ChatViewController {
  let inputView = InputView()

  override var canBecomFirstResponder: Bool { true }
  
  override var inputAccessoryView: UIView? { inputView } 
  
  override func viewDidAppear(_ animated: Bool) {
    super.viewDidAppear(animated)
    becomeFirstResponder()
  }
}
```

***Notes:***

Even this is the recommended way to do these kind of user interfaces, I sill see a bunch of warnings and errors.
- By returning `.zero` for the `intrinsicContentSize`, we will get this error: `API error: <_UIKBCompatInputView: 0x107e849a0; frame = (0 0; 0 0); layer = <CALayer: 0x280ff7900>> returned 0 width, assuming UIViewNoIntrinsicMetric`
- First responder warning `rejected resignFirstResponder when being removed from hierarchy`
- Snapshotting a view (0x12838cc10, _UIReplicantView) that has not been rendered at least once requires afterScreenUpdates:YES.

The last one may be an issue of the keyboard but I don't see the first 2 issues when we are not using the `inputAccessoryView`.
The fact that the `inputAccessoryView` is not in the same view hierarchy as the main app, so debugging this is not easy. 
For now, I haven't found a good solution for these issue.

***Fun fact***

All of the 3 issues above also happen in Apple apps (Messages, Notes, ...)

