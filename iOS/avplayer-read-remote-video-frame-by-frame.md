## Fetching frame by frame for further processing of a remote video by using AVPlayer

For some advance cases such as adding custom filters into a video while playback,
you may want to customize the way the video is rendered on screen.
AVPlayer is super flexible since it allows us to do almost any thing by giving us 
all the fetched frames one by one, then we can do whatever we want with those single frames 
for example, we can apply some CIFiters or devide each frame into multiple rectangles 
and render them at arbitary locations on the screen.

## Fetching video data frame by frame while playback

```Swift
let displayLink = CADisplayLink(target: self, selector: #selector(loop))
displayLink.add(to: RunLoop.main, forMode: .default)

var player = AVPlayer()
var output = AVPlayerItemVideoOutput()

let item = AVPlayerItem(asset: AVAsset(url: url))
item.add(output)

player.replaceCurrentItem(with: item)
player.play()

@objc func loop() {
    let itemTime = self.output.itemTime(forHostTime: CACurrentMediaTime())

    var presentationItemTime = CMTime.zero
    if let frame = self.output.copyPixelBuffer(forItemTime: itemTime, itemTimeForDisplay: &presentationItemTime) {
        let ciImage = CIImage(cvPixelBuffer: frame)
        // Now you can do what ever you want with the CIImage
    }
}
```

