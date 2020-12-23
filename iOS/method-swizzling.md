## Method Swizzling

Method Swizzling helps us to change the implementation of a method at runtime.

Method Swizzling requires:
- The class must extend `NSObject`
- The method must have the `dynamic` modifier

***You shoud NOT use the `@objc` attribute because it only makes your Swift API 
available in Objective-C and the Objective-C runtime but it does NOT guarantee 
dynamic dispatch which is required by method swizzling***

This feature is commonly used in analytics frameworks.

Generic implementation:
- Get the implementation of the original method
- Get the implementation of the the method that will replace the original one
- Swap 2 implementation

```Swift
func swizzling(_ c: AnyClass,_ s1: Selector,_ s2: Selector) {
  guard
    let m1 = class_getInstanceMethod(c, s1),
    let m2 = class_getInstanceMethod(c, s2)
  else { return }

  method_exchangeImplementations(m1, m2)
}
```

Usuage:

```Swift
extension UIWindow {
  static func swiizzle() {
    let originalSelector = #selector(sendEvent(_:))
    let swizzledSelector = #selector(swizzled_sendEvent(_:))
    swizzling(UIWindow.self, originalSelector, swizzledSelector)
  }

  @objc func swizzled_sendEvent(_ event: UIEvent) {
    swizzled_sendEvent(event)
    print("swizzled_sendEvent")
  }
}
```

You may confused whether it will end up in an infinite loop or ot. 
Fortunately, it will not. The reason is, calling `swizzled_sendEvent` 
after swizzling means your are calling to the original implementation one.

Be careful, calling `swizzled_sendEvent` before swizzling will create an infinite loop.
