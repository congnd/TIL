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
