### Debug Autolayout

- Create symbolic breakpoint: Debug -> Breakpoints -> Create symbolic breakpoints...
- Add `UIViewAlertForUnsatisfiableConstraints` into the symbolic field

When debug, run this to get more infomation
- Objc-C project: `po [[UIWindow keyWindow] _autolayoutTrace]`
- Swift project: `expr -l objc++ -O -- [[UIWindow keyWindow] _autolayoutTrace]`


Ref: http://web.archive.org/web/20180311113608/http://nshint.io/blog/2015/08/17/autolayout-breakpoints/
