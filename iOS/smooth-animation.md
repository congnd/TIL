### Docs
- Explore UI animation hitches and the render loop: https://developer.apple.com/videos/play/tech-talks/10855
- Find and fix hitches in the commit phase: https://developer.apple.com/videos/play/tech-talks/10856/
- Demystify and eliminate hitches in the render phase: https://developer.apple.com/videos/play/tech-talks/10857/

- High performance Auto Layout: https://www.wwdcnotes.com/notes/wwdc18/220/
- Image and graphics best practices: https://developer.apple.com/videos/play/wwdc2018/219

### Testing & Analysis
- MetricKit: https://developer.apple.com/videos/play/wwdc2020/10081/
- Eliminate animation hitches with XCTest: https://developer.apple.com/videos/play/wwdc2020/10077/

### Debugging & Tips
- Show layers while debugging the view hierarchy:  *Editor* -> *Show layers*
- Show opimization oppotunities: *Editor* -> *Show optimization oppotunities*
- `layer.cornerCurve = .continuous` to get smooth rounded corner rather than creating an UIBezierPath (https://stackoverflow.com/a/59993994/3867033).
This also reduces 2 Offscreen count.

### Commit phase
4 smaller phases:
- Layout: `layoutSubviews`
- Display: `draw(rect:)` trigged by 1. adding views that overrdie `draw(rect:)` or 2. call `setNeedsDisplay`
- Prepare: image decoding, etc...
- Commit: package up view layer tree and send to the render server

### Render phase
2 smaller phases: 
- Render prepare: break down animations, break down layers and effect into step-by-step plan of simple operations
- Render execute: draws each step using GPU, complies the final image for display

#### Offscreen Pass
Anytime the GPU must render a layer by first rendering it somewhere else, then copying it over. 
**Offscreen Passes** can cause hitches, and we should avoid them.

4 main types of Offscreen Pass:
- Shadowing
- Masking
- Rounded rectangles
- Visual effects
