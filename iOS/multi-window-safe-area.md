### Safe area in multi windows applications

When a layout update happens, which may cause safe area change, UIKit notifies the changes 
to the key window ONLY. But when the app entered foreground, UIKit tries to update 
all the current active windows in the app. Which sometime may lead to some weird layout issues.
To solve the issue, we create a subclass of UIWidow which refuses all the changes to its 
`safeAreaInsets` when it's NOT the key window.

```Swift
/// The window that refuses to update it's internal safeAreaInsets
/// when it's not the key window of the application.
///
/// This is necessary since iOS updates the safeareainset of all the current active windows
/// when app enters foreground mode but does NOT do the same when rotation in foreground.
public class SafeWindow: UIWindow {
    private var isMakeKeyAndVisibleInProgress: Bool = false
    private var _safeAreaInsets: UIEdgeInsets = .zero

    /// It seems that the `safeAreaInsetsDidChange` depends on this one to detect changes,
    /// so relying in that method to update the internal safe area insets may not work as expected.
    public override var safeAreaInsets: UIEdgeInsets {
        if isKeyWindow || isMakeKeyAndVisibleInProgress {
            _safeAreaInsets = super.safeAreaInsets
        }
        return _safeAreaInsets
    }

    public override func makeKeyAndVisible() {
        isMakeKeyAndVisibleInProgress = true
        super.makeKeyAndVisible()
        isMakeKeyAndVisibleInProgress = false
    }
}
```

___Update (2021/02/08):___
Today, I realized that the navigation bar frame get update even with the above fix.
So I guess there is a machenism that allows those components to be updated when app entering foreground.
This doesn't seem appearing on all device models but only some specific ones.
