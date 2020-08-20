### Load custom fonts inside dynamic frameworks

If all you need is just adding custom fonts into the main modules, the you can use this method

https://developer.apple.com/documentation/uikit/text_display_and_fonts/adding_a_custom_font_to_your_app

But when you want to add and use custom fonts in your frameworks, the solution above doesn't work for you.

`CTFontManagerRegisterFontsForURL` comes to rescue.

https://developer.apple.com/documentation/coretext/1499468-ctfontmanagerregisterfontsforurl

```Swift
CTFontManagerRegisterFontsForURL(fontUrl1 as CFURL, .process, nil)
CTFontManagerRegisterFontsForURL(fontUrl2 as CFURL, .process, nil)
```
