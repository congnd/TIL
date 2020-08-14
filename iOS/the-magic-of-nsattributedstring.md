We can use `NSAttributedString` to decode HTML entites encoded strings.

We also can use `NSAttributedString` to convert HTML string into a NSAttributedString.

And the process for those are exactly the same. Only the input strings are different.

```Swift
let html1 = """
&lt;span class=&quot;st&quot;&gt;&lt;em&gt;Bread&lt;/em&gt; is a staple food, usually by baking. Throughout ... &lt;em&gt;Sourdough&lt;/em&gt; is a type of &lt;em&gt;bread&lt;/em&gt; produced by dough using naturally occurring yeasts and lactobacilli. ... List of &lt;em&gt;toast&lt;/em&gt; dishes&lt;/span&gt;
"""

extension String {
  var toAttributedString: NSAttributedString? {
    return try? NSAttributedString(
      data: data(using: .utf8)!,
      options: [
        .documentType: NSAttributedString.DocumentType.html,
      ],
      documentAttributes: nil)
  }
}

let output1 = html1.toAttributedString!.string 
// <span class="st"><em>Bread</em> is a staple food, usually by baking. Throughout ... <em>Sourdough</em> is a type of <em>bread</em> produced by dough using naturally occurring yeasts and lactobacilli. ... List of <em>toast</em> dishes</span>

let output2 = output1.toAttributedString
// styled version for the above string.
```
