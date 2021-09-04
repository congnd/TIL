## A better way to use AVPlayerLayer

### Problem
I've been using AVPlayerLayer as a sublayer of a view's layer for a long time until to day, 
I faced with a problem where my AVPlayerLayer doesn't animate synchronously with it's super layer's bounds.

```Swift
final class PlaybackContentView: UIView {
  var player: AVPlayer? {
    get { playerLayer.player }
    set { playerLayer.player = newValue }
  }
  private let playerLayer = AVPlayerLayer()

  init() {
    super.init(frame: .zero)
    backgroundColor = .black
    layer.addSublayer(playerLayer)
  }

  override func layoutSubviews() {
    super.layoutSubviews()
    playerLayer.frame = layer.bounds
  }
}

```

After googling a bit, I realized that every time I change the `frame` on my AVPlayerLayer it seems that
AVKit adds a slight animation onto my layer. This can be helpful in most cases but when we need more customize capability, it might be painful 

### Solution
I don't really understand the reason behind but if we let UIKit know that AVPlayerPlayer is the 
layer class we want to use as the backbone for a custom view instead of a general CALayer, the automatically added animation described above gone away.
And now you can fully control how your view (or you can say you AVPlayerLayer) can be animated.

```Swift
final class ContentView: UIView {
  var playerLayer: AVPlayerLayer! { layer as? AVPlayerLayer }

  var player: AVPlayer? {
    get { playerLayer.player }
    set { playerLayer.player = newValue }
  }

  override class var layerClass: AnyClass {
    AVPlayerLayer.self
  }

  init() {
    super.init(frame: .zero)
    backgroundColor = .black
    layer.masksToBounds = true
  }
}
```

After checking the documentation of AVPlayerLayer, I know that I've missed something important so long. LOL
Apple already have a code example on how to use AVPlayerLayer and interestingly, it's exactl the same with the solution above.
