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

### With exact the same configuration as on Figma, shadow on iOS doesn't look like what it should be
When trying to implement a shadow style from Figma, you should devide the blur value set on Figma by 2 
then use that value to set to the `shadowRadius` of CALayer. Not sure the reason behind this.

### By default, UIGestureRecognizer cancels touches when it detected the gesture
If you to handle touches even after a particular gesture recognizer has detected a gesture, 
you can set the `cancelsTouchesInView` to `false`

### dSYM uploading for crash report tools
- Make sure that you are generating dSYM files when building the app even for archive or development
- You can check for the generated DSYM files in the product folder of your project
- Each target has its own debug symbols file, so be sure to upload all of them
- Use the metadata find command `mdfind` to find the missing UUID
- Show metadata of a file `mdls path/to/file`

### Running low on storage space in your Mac?
- Remove all unsupported simulators.
```
xcrun simctl delete unavailable
```

- Remove simulators for old iOS versions you no longer need.
```
cd /Library/Developer/CoreSimulator/Profiles/Runtimes
sudo rm -rf iOS\ 12.1.simruntime/
```

### Can not find XXX when compiling project using `import Firebase`

Detail is written in this issue https://github.com/firebase/firebase-ios-sdk/issues/6066.
One of the possible solution here: https://github.com/firebase/firebase-ios-sdk/issues/6066#issuecomment-662580211

But I also solved the issue by removing all the content of the `Pods/Firebase/CoreOnly/Firebase.h`, compile, undo the change and then compile again.

I suspect this happens because of the caching and compiling order.
