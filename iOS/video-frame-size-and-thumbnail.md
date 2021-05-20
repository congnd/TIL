## Get frame size and a thumbnail of a video

```Swift
extension AVURLAsset {
  var size: CGSize {
    var size = CGSize(width: 400, height: 400)
    if let track = tracks(withMediaType: .video).first {
      size = track.naturalSize.applying(track.preferredTransform)
      size = CGSize(width: abs(size.width), height: abs(size.height))
    }
    return size
  }

  func getFirstFrame() -> UIImage? {
    let imageGenerator = AVAssetImageGenerator(asset: self)
    imageGenerator.appliesPreferredTrackTransform = true
    var thumbnail: UIImage?

    let firstMoment = CMTime(seconds: 0, preferredTimescale: 1)
    if let cgImage = try? imageGenerator.copyCGImage(at: firstMoment, actualTime: nil) {
      thumbnail = UIImage(cgImage: cgImage)
    }
    return thumbnail
  }
}
```
