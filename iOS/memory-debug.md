## Memory debug

### Get retain count
https://developer.apple.com/documentation/corefoundation/1521288-cfgetretaincount
```
po CFGetRetainCount($0)
```

### Increase and decrease retain count
https://developer.apple.com/documentation/corefoundation/1521269-cfretain
https://developer.apple.com/documentation/corefoundation/1521153-cfrelease
```
CFRetain($0)
CFRelease($0)
```
