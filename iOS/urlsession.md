### Get response data from server when using method which does not provide callback handler

When using method like `urlSession.uploadTask(withStreamedRequest:)`, you have no chance to provide callback handler 
which usually comes with response data from server. So what do we do in this case?

Well, it's very simple. In fact, the callback handler if exist is just a wrapper around some existing delegate methods 
which will be called by the url session. And here is the solution:

```Swift
var responseData = Data()

public func urlSession(
  _ session: URLSession,
  task: URLSessionTask,
  didCompleteWithError error: Error?
) {
  if let response = task.response as? HTTPURLResponse {
    print("response code: \(response.statusCode)")
    print("response body: \(String(data: responseData, encoding: .utf8))")
  }
}

public func urlSession(
  _ session: URLSession,
  dataTask: URLSessionDataTask,
  didReceive data: Data
) {
  responseData.append(data)
}
```

### Multipart Form Data

URLSession comes with a very handy solution for almost all complicated client-server communication scenarios - `Stream` data structure.
In this secion, I'm going to focus on how to use URL session to upload a huge amount of data which can't be loaded into memory at a time 
such as like a large local file.

Since URL has no easy to use multipart form data functionality, we can build our own solution based on `InputStream` and `OutputStream`.

Firstly, we need to prepare our whole body data and write it to a temporary file.

```Swift
typealias MultipartData = (name: String, fileName: String, stream: InputStream)

func writeFormDataToFile(multipartData: [MultipartData], boundary: String) -> URL? {
  let fileManager = FileManager.default
  let tempDirectoryURL = FileManager.default.temporaryDirectory
  let directoryURL = tempDirectoryURL.appendingPathComponent("com.congnd/multipart.data")
  let fileName = UUID().uuidString
  let fileURL = directoryURL.appendingPathComponent(fileName)

  do {
    try fileManager.createDirectory(
      at: directoryURL,
      withIntermediateDirectories: true,
      attributes: nil
    )

    if let fileStream = OutputStream(url: fileURL, append: false) {
      fileStream.open()
      defer { fileStream.close() }

      multipartData.forEach { aPart in
        let headerStream = header(boundary: boundary, name: aPart.name, filename: aPart.fileName)
        write(input: headerStream, to: fileStream)
        write(input: aPart.stream, to: fileStream)
      }

      let ending = InputStream(data: "\r\n--\(boundary)--\r\n".data(using: .utf8)!)
      write(input: ending, to: fileStream)
    }

    return fileURL
  } catch {
    try? fileManager.removeItem(at: fileURL)
    return nil
  }
}

func randomBoundary() -> String {
  let first = UInt32.random(in: UInt32.min...UInt32.max)
  let second = UInt32.random(in: UInt32.min...UInt32.max)

  return String(format: "com.congnd.%08x%08x", first, second)
}

func write(input: InputStream, to output: OutputStream) {
  input.open()
  defer { input.close() }

  while input.hasBytesAvailable {
    var p = [UInt8](repeating: 0, count: 1024)
    var bytesToWrite = input.read(&p, maxLength: 1024)

    while bytesToWrite > 0 {
      let written = output.write(p, maxLength: bytesToWrite)
      bytesToWrite -= written

      if bytesToWrite > 0 {
        p = Array(p[written..<p.count])
      }
    }
  }
}

func header(boundary: String, name: String, filename: String) -> InputStream {
  let header = """
    --\(boundary)\r
    Content-Disposition: form-data; name=\"\(name)\"; filename=\"\(filename)\"\r
    Content-Type: application/octet-stream\r\n\r\n
    """

  let data = header.data(using: .utf8)!
  return InputStream(data: data)
}
```

Then use the temporary file to create an upload task.

```Swift
let fileUrl = writeFormDataToFile(multipartData: data, boundary: boundary)!
urlSession.uploadTask(
  with: urlRequest,
  fromFile: fileUrl,
  completionHandler: completion
)
```
