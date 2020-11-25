## Changing the size of a paging scroll

You may want to use UIScrollView or its subclasses to enable the paging feature 
but you'll soon realize that sometimes the designer want to show not only the current page at a time 
but also a small area of the next/prev page in order to suggest users that there are some other pages.

There is no one-line solution for the problem above, but UIScrollView somehow allows you to do that.
To do so, you need to modify some default implemetataion of the UIScrollView. 

The page size of the UIScrollView is always equal to its bound size, and there is no way to directly modify that size.
Therefore, we are going to:

1. make the bound size of the UIScrollView the intended size
2. set the `clipToBounds` to `false` to allow the area outside of the bounds area can be visible

Because you've made the scrollview smaller than the visible/pannable area so you need to fix that issue. Fortunately, the `panGestureRecognizer` comes to rescue.

3. create a new UIView with the size equal to the size of the visiable area of your page (not the size of UIScrollView) and put that view over the scrollview
4. move the `panGestrureRecognizer` from scroll view to your new UIView created in the step 3 using `UIScrollView.removeGestureRecognizer(:)` and then `UIView.addGestureRecognizer(:)`

Now you'll have exactly the same UI that you want to create without any trade-off.
