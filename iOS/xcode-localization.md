### Change the development localization
- Open the Info.plist file and change the `Localization native development region` to the region you want.
- Open the `.pbxproj` by a normal coder editor like VS Code, and change the the `developmentRegion` to the region you select above.
- Close and open your project by Xcode again and you should see Xcode displays your desired region as development language. This also will be used as a fallback language.

### What is the undeletable `Base` localization
- Along with the development localization, those are 2 item that Xcode does NOT allow you to delete.
- `Base` is not actually a __localization__. Instead, it's used to holds xibs and storyboards that you want to localize.
- `Base.lproj` contains only xibs and storyboards
- By default, all newly created xibs or storyboards are saved in the `Base.lproj`. You can choose to not enable `Base` for a particular xib or storyboard but besure that you provide all version of that file for all language you want to support. Otherwise, your app might crash when you choose that langauge that does has the required xib/storyboard.
- Enabling `Base` for xibs/storyboards and using string files to provide translations for those xibs/storyboards are the recommend.

Refer:
https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html#//apple_ref/doc/uid/10000171i-CH3-SW2
