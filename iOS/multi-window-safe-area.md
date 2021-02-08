### Safe area in multi windows applications

When a layout update happens, which may cause safe area change, UIKit notifies the changes 
to the key window ONLY. But when the app entered foreground, UIKit tries to update 
all the current active windows in the app. Which sometime may lead to some weird layout issues.
To solve the issue, we create a subclass of UIWidow which refuses all the changes to its 
`safeAreaInsets` when it's NOT the key window.

```Swift
public class Window: UIWindow {
  private var _safeAreaInsets: UIEdgeInsets = .zero

  public override func safeAreaInsetsDidChange() {
    if isKeyWindow || UIWindow.key == nil {
      _safeAreaInsets = super.safeAreaInsets
    }
  }

  public override var safeAreaInsets: UIEdgeInsets {
    if isKeyWindow || UIWindow.key == nil {
      return super.safeAreaInsets
    }
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

___Update (2020/02/08):___
Today, I realized that the navigation bar frame get update even with the above fix.
So I guess there is a machenism that allows those components to be updated when app entering foreground.
This doesn't seem appearing on all devoce models but only some specific ones.
