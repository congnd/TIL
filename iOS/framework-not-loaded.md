## Framework/library not loaded

Have you ever seen an error like this?

```
Assertions: System: Failed to load the test bundle. If you believe this error represents a bug, please attach the result bundle at xyz.xcresult. 
(
  Underlying Error: The bundle “MyAppApiTests” couldn’t be loaded because it is damaged or missing necessary resources. 
  The bundle is damaged or missing necessary resources. Try reinstalling the bundle. 
  dlopen_preflight(MyAppApiTests): Library not loaded: @rpath/Apollo.framework/Apollo
  Referenced from: MyAppApi
  Reason: image not found
)
```

As the error message suggests, it seems that you don't have the Apollo framework in your final bundle.
Possibly, you've forgotten to copy the framework to your bundle (by embed framwork or by carthage-copy if you are using Carthage).
The reason why Xcode may not report any error while compiling is, probably, 
you've set the framework search path in your Project Setting to the location which contains your library.

If you are importing the framework directly from you test target (not recommend), make sure that, 
by some way, you have the framework in your built test bundle. If not, the same error may occur.
