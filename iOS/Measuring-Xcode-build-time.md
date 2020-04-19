# Measuring Xcode build time

## Using Xcode build report
You can easily get the build time report from Xcode by navigating to the Report Navigator.
Further more, you can also force Xcode to show the total build time in the Xcode Activity Viewer by running this command line:
```Bash
defaults write com.apple.dt.Xcode ShowBuildOperationDuration YES
```

## Showing Xcode Build Timing Summary
- By selecting: `Product->Perform Action->Build With Timing Summary`
- By using command line:

*For clean builds*
```Bash
xcodebuild -project 'Kickstarter.xcodeproj' \
-scheme 'Kickstarter-iOS' \
-configuration 'Debug' \
-sdk 'iphonesimulator' \
-showBuildTimingSummary \
clean build | sed -n -e '/Build Timing Summary/,$p'
```

*For incremenatl builds*
```Bash
touch /path/to/a/swift/source/file.swift && \ # Make change to a file to force Xcode rebuild
xcodebuild -project 'Kickstarter.xcodeproj' \
-scheme 'Kickstarter-iOS' \
-configuration 'Debug' \
-sdk 'iphonesimulator' \
-showBuildTimingSummary \
build | sed -n -e '/Build Timing Summary/,$p'
```

## Get some insights about type-checking time
- Go Project Setting -> Other Swift Flags
- Add the following flags:

```Bash
-Xfrontend -warn-long-function-bodies=100
-Xfrontend -warn-long-expression-type-checking=100
```

## More diagnostic options
https://github.com/apple/swift/blob/master/docs/CompilerPerformance.md#diagnostic-options

- Use -debug-time-compilation flag to get the top slowest files to compile
```Bash
xcodebuild -project 'Kickstarter.xcodeproj' \
-scheme 'Kickstarter-iOS' \
-configuration 'Debug' \
-sdk 'iphonesimulator' \
clean build \
OTHER_SWIFT_FLAGS="-Xfrontend -debug-time-compilation" |
    awk '/CompileSwift normal/,/Swift compilation/{print; getline; print; getline; print}' |
    grep -Eo "^CompileSwift.+\.swift|\d+\.\d+ seconds" |
    sed -e 'N;s/\(.*\)\n\(.*\)/\2 \1/' |
    sed -e "s|CompileSwift normal x86_64 $(pwd)/||" |
    sort -rn |
    head -3

25.6026 seconds Library/ViewModels/SettingsNewslettersCellViewModel.swift
24.4429 seconds Library/ViewModels/PledgeSummaryViewModel.swift
24.4312 seconds Library/ViewModels/PaymentMethodsViewModel.swift
```

-  List top slowest function bodies and expressions in the Type Checking stag
```Bash
xcodebuild -project 'Kickstarter.xcodeproj' \
-scheme 'Kickstarter-iOS' \
-configuration 'Debug' \
-sdk 'iphonesimulator' \
clean build \
OTHER_SWIFT_FLAGS="-Xfrontend -debug-time-expression-type-checking \
    -Xfrontend -debug-time-function-bodies" |
  grep -o "^\d*.\d*ms\t[^$]*$" |
  awk '!visited[$0]++' |
  sed -e "s|$(pwd)/||" |
  sort -rn |
  head -5

16226.04ms	Library/Styles/UpdateDraftStyles.swift:31:3
10551.24ms	Kickstarter-iOS/Views/RewardCardContainerView.swift:171:16	instance method configureBaseGradientView()
10547.41ms	Kickstarter-iOS/Views/RewardCardContainerView.swift:172:7
8639.30ms	Kickstarter-iOS/Views/Controllers/AddNewCardViewController.swift:396:67
8233.27ms	KsApi/models/templates/ProjectTemplates.swift:94:5
```

## Build time analyzer
Use `XCLogParser` to aggregate all the metrics above.

https://github.com/spotify/XCLogParser

________________________________________________
Source: https://www.onswiftwings.com/posts/build-time-optimization-part1/