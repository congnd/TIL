### Debug Autolayout

- Create symbolic breakpoint: Debug -> Breakpoints -> Create symbolic breakpoints...
- Add `UIViewAlertForUnsatisfiableConstraints` into the symbolic field

When debug, run this to get more infomation
- Objc-C project: `po [[UIWindow keyWindow] _autolayoutTrace]`
- Swift project: `expr -l objc++ -O -- [[UIWindow keyWindow] _autolayoutTrace]`

___Bonus:___ change background color in debug 
```
(lldb) expr ((UIView *)0x7f9ea3d43410).backgroundColor = [UIColor redColor]
(UICachedDeviceRGBColor *) $1 = 0x00007f9ea3d43410
```

Ref: http://web.archive.org/web/20180311113608/http://nshint.io/blog/2015/08/17/autolayout-breakpoints/
