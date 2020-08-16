### Keep command line tool running until async operations finish

Use `Semaphore`.

```Swift
let semaphore = DispatchSemaphore(value: 0)

performAsyncOperation {
    print("Done")
    semaphore.signal()
}

semaphore.wait()
```
