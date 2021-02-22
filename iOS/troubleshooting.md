## Troubleshooting

### dyld: Library not loaded: @rpath/xxx.framework

This happens when the dynamic framework loader could not find a required framework at startup time,
and may arise by forgetting to embed your frameworks into the final bundle. 
This can be done via General tab -> Frameworks section, `Carthage` uses a script to copy 
your frameworks into app bundle at build time)

If you see this specific message: `dyld: Library not loaded: @rpath/XCTest.framework/XCTest`
then probably, you are embedding frameworks for test purpose only (RxSwift has RxTest, RxBlocking)
, that require `XCTest` internally, into your app bundle. The solution for this is just remove 
those frameworks from General -> Frameworks section of your app target.

### no such module 'ABC'

A very common issue and can be caused by many reasons. 
One of the reason that I've just encountered is, while working with xcframeworks built by Carthage,
it's okay to compile the project on Xcode GUI without declaring the framework that we reference to
in a non-main module by adding them with `Do not embed` option in the Frameworks section of the module.
But this becomes a problem when using command line tool. For example, I'm using fastlane's `build_app`
action, and it gave me the error above. So my recommendation here is, always declare the frameworks
that you want to use in the Framework section of target module.

### Auto-sizing tableview

Sometimes, it's necessary to dinamically update cell's constraints based on the passed data.
In that case, it's better to:
- always set priority for one of vertical constraints to 999 to suppress autolayout warning 
because of the `UIView-Encapsulated-Layout-Height`
- call `layoutIfNeeded` on the cell after changing its constraints/content

### error: exportArchive: IPA processing failed
```
error: exportArchive: IPA processing failed
Error Domain=IDEFoundationErrorDomain Code=1 "IPA processing failed" UserInfo={NSLocalizedDescription=IPA processing failed}
```
Have you seen this error when trying to build your app?
For my case, it happened because I added the same library into 2 module and both of them are congired with Embedded and Sign.
Revise configuration fixed the issue (only set Embedded and Sign for the app module, for other modules, use Do NOT embed)
