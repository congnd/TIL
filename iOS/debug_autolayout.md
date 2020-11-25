### Debug Autolayout

- Create symbolic breakpoint: Debug -> Breakpoints -> Create symbolic breakpoints...
- Add `UIViewAlertForUnsatisfiableConstraints` into the symbolic field

When debug, run this to get more infomation
- Swift project: `expr -l objc++ -O -- [[UIWindow keyWindow] _autolayoutTrace]`
- Objc-C project: `po [[UIWindow keyWindow] _autolayoutTrace]`

If you want to cast raw address into a specific type:
```Swift
expr -l Swift -- import UIKit
expr -l Swift -- let $view = unsafeBitCast(0x7df67c50, to: UIView.self)
expr -l Swift -- print($view.alpha)
```

___Bonus:___ change background color in debug 
```
(lldb) expr ((UIView *)0x7f9ea3d43410).backgroundColor = [UIColor redColor]
(UICachedDeviceRGBColor *) $1 = 0x00007f9ea3d43410
```

Refs:

http://web.archive.org/web/20180311113608/http://nshint.io/blog/2015/08/17/autolayout-breakpoints/
https://stackoverflow.com/a/42766226/3867033
