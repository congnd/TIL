## Date format - The difference beween `Y` and `y`

- `y`: Year - just as what you expect normally
- `Y`: Year (in "Week of Year" based calendars). This may not always be the same value as the `y`.

Lock at the calendar below (`cal Jan 2020`), Jan first of this year can be considered as the the first week of 2020
or the 53rd week of the last year - 2019.

If you want to create a new Date using the year (in `Week of Year` based calendars) you may want to provide
2 other parameter to provide sufficient data to create any date. Using `e` for the day of the week 
and the `ww` for the orfinal week of the year. Both `ww` and `e` start from `1` but not `0`.

```
   December 2019
Su Mo Tu We Th Fr Sa
 1  2  3  4  5  6  7
 8  9 10 11 12 13 14
15 16 17 18 19 20 21
22 23 24 25 26 27 28
29 30 31

    January 2020
Su Mo Tu We Th Fr Sa
          1  2  3  4
 5  6  7  8  9 10 11
12 13 14 15 16 17 18
19 20 21 22 23 24 25
26 27 28 29 30 31
```

For example:
```
let formatter = DateFormater()
formatter = "ww-e-YYYY"

formatter.date(from: "01-1-2020") // Output: 2019/12/29
formatter.date(from: "53-4-2019") // Output: 2020/01/01
```

If you do not provide the `ww` and `e`, then they will be treated as the the zeroth week (the week before the first week)
and the first day of that week. For example:

```
let formatter = DateFormater()
formatter = "YYYY"

formatter.date(from: "2020") // Output: 2019/12/22
```

An interesting point here is, if you specify the week and the day explictly, like the above example, then you receive the `nil` value (as of iOS12).

```
let formatter = DateFormater()
formatter = "ww-e-YYYY"

let s = formatter.date(from: "00-0-2020") // output: nil
let s1 = formatter.date(from: "01-0-2020") // output: nil
let s2 = formatter.date(from: "00-1-2020") // output: nil

let s3 = formatter.date(from: "01-1-2020") // output: 2019/12/29
```

1. https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html
1. http://www.unicode.org/reports/tr35/tr35-31/tr35-dates.html#dateTimeFormats
2. https://juejin.im/entry/6844903747982753799
