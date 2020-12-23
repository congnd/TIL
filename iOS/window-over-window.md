## Window over window

Majority of applications out there doesn't need another window diplay on the main window. 
but sometimes, for example, when you want to implement the PIP (picture-in-picture) within your app 
then you probably need to have another window that holds your PIP layer to ensure that 
there is no other view can be displayed on top of the PIP.

Here is a very simple example that show you how to make a new window and make it visible on the screen.

```
let myWindow = UIWindow() // #1
myWindow.rootViewController = UIViewController() // #2
myWindow.frame = UIScreen.main.bounds // #2
myWindow.frame = UIWindow.Level.statusBar + 1
myWindow.isHidden = false // #3
```

***This may not work in UIScene-enabled application. You have to remove all scene configurations.***

- ***#1***. Create an instance of the UIWindow. You can subclass UIWindow to do things you want. 
Since UIWindow is just a subclass of the UIView, you can do whatever you want with that view.

- ***#2***. UIWindow requires a view controller as the rootViewController.
Since we can not use autolayout for the window because it does not have a superview like a normal UIView, 
we need to handle stuffs like orientation ourself. And of course, you can do it with the root view controller above.
If you don't need any customization, just create an UIViewController and set it to the root view controller of the window.

- ***#3***. By default, your newly created window is not added to the screen so you need to make it visible by setting the `isHidden` to false
