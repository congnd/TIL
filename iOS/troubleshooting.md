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
