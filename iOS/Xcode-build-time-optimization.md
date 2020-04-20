# Xcode build time optimization

## Measuring build time first
[Measuring-Xcode-build-time.md](/iOS/Measuring-Xcode-build-time.md)

### Build setting optimizations
For debug mode (incremental builds) we should:
- Set the `Build Active Architecture Only` to `YES`
- Set the `Compilation Mode` to `Incremental`, release builds should be set to `Whole Module`
- Set the `Optimization Level` to `No optimization`
- Set the `Debug Information Format` to `DWARF` (***without*** dSYM file)

### Source code improvements
- Use this tool to know what are offenders: https://github.com/RobertGummesson/BuildTimeAnalyzer-for-Xcode
- Specify explixit datatype for complicated expressions
- Define entities in separate files
- Use proper access specifiers

________________________________________________
Source: https://www.onswiftwings.com/posts/build-time-optimization-part2/