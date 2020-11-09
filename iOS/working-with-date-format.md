## Working with date format

When working with date format, there is a commom misunderstanding when people do not really care about the `Calendar`, `Locale` and `TimeZone`.
Those are very important configurations which produces different output for a same data instance. 
And because your users can freely change those setting in the Setting app, you app may not work consistant across users over the world.

### Calendar
If you create a formatter like this:
```
let formatter = DateFormatter()
formatter.dateFormat = "yyyy-MM-dd"
```

And then, you run this code on a system that is currently set to use `Gregorian` calendar, 
then you'll get what you've expected which is `2020/11/10`.
```
formatter.date(from: "2020-11-10")
```
But if you run the same code but on a system that is using the `Japanese` calendar, 
the you'll get a very wierd value `4008-12-01`.

Therefore, you may not want to depend on the default calendar which is the system calendar, 
but instead setting the `calendar` property for your formatter.

### Locale
To be honest, I'm not sure how the local affects the the procuded output String/Date of the formatter.
But you may see the difference if you try these 2 cases:
- run the code below in a system which uses `Gregorian` calendar and the time format is set to `24-Hour` style.
- run the code below in a system which uses `Gregorian` calendar and the time format is set to `12-Hour` style.

```
let formatter = DateFormatter()
formatter.dateFormat = "yyyy-MM-dd HH:mm"

let date = Date("2020-12-01 10:00")
```

On the `24-Hour` style system:
```
let s = formatter.date(from: "2020/12/01 10:00") // Output: 2020/12/01 10:00
```

But on the `12-Hour` style system:
```
let s = formatter.date(from: "2020/12/01 10:00") // Output: nil
let s = formatter.date(from: "2020/12/01 午前10:00") // Output: 2020/12/01 10:00 if you set to the `ja_JP` region
```

And depending on your current region, the string procuded by the formatter may vary:

```
let d = formatter.string(from: date) // Output: // Output: 2020-12-01 午後10:00 if you set to the `ja_JP` region
let d = formatter.string(from: date) // Output: // Output: 2020-12-001 10:00 if you set to the `ja_US` region
```

***The hour field is not affected by the `24-Hour` style setting if you change the uppercased `H` to the lowercased `h`***

The recommendation here is, always set the locale on your formatter to the `en_US_POSIX` 
to ensure your app run correctly acroess users from all over the world.

### TimeZone
Don't the the above 2 configurations, timezone is quite easy to understand and use.
By default, you formatter use the current system timezone so make sure that 
you set the `timzezone` on your formatter to a proper timezone before parsing any date string from outside.


### Conclusion
Depends on your purpose of using DateFormater but in most of the cases, your DateFormatter should look like this:

```
let formatter = DateFormatter()
formatter.calendar = yourDesireCalendar
formatter.local = yourDesiredLocale
formatter.timeZone = yourDesiredTimezone

// And then, set your date format
formatter.dateFormat = "YOUR-FORMAT"

```

