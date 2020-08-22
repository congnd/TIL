### `CALayer`'s `contents` property on iOS 14

Prior to iOS14, the `contents` is initialized right after you set image for the UIImageView.
So you can use the `contents` property latter to do whatever you want, for example [taking snapshot](https://github.com/congnd/FMPhotoPicker/blob/master/FMPhotoPicker/FMPhotoPicker/source/Utilities/UIView%2BFMPhotoView.swift#L12).

But start from iOS 14, the `contents` is not initialized until the UIImageView is added to the view's hierarchy 
or after the `snapshotView(afterScreenUpdates: true)` is called on the UIImageView.

So what do you do when you need to get the `contents` before the view is added to the view's hierarchy?

### CALayer's `displayIfNeeded` comes to rescue

You can force iOS to inititate the `contents` before you want to use it.

```Swift
if myImageView.contents == nil {
  myImageView.setNeedsDisplay()
  myImageView.displayIfNeeded()
}
```
