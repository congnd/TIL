### Safe area in multi windows applications

When a layout update, which causes safe area change, happened, UIKit notifies the changes 
to the key window only but not all the windows in the app. But when the app entered 
foreground, UIKit updates all the current active windows in the app. Which, sometime,
may lead to some weird layout issues. To solve the issue, we create a subclass of UIWidow 
which refuses all the changes to its `safeAreaInsets` when it's NOT the key window.

```Swift
public class Window: UIWindow {
  private var _safeAreaInsets: UIEdgeInsets = .zero

  public override func safeAreaInsetsDidChange() {
    if isKeyWindow || UIWindow.key == nil {
      _safeAreaInsets = super.safeAreaInsets
    }
  }

  public override var safeAreaInsets: UIEdgeInsets {
    return _safeAreaInsets
  }
}

extension UIWindow {
  static var key: UIWindow? {
    if #available(iOS 13, *) {
      return UIApplication.shared.windows.first { $0.isKeyWindow }
    } else {
      return UIApplication.shared.keyWindow
    }
  }
}
```
