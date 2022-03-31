## Change name of files in a folder

```js
function changeName() {
  const baseVideo = './public/video/'
  const videoReg = new RegExp(/^Fragments\(video%3d(.*)%2cformat%3dm3u8-aapl\)$/)

  const baseAudio = './public/audio/'
  const audioReg = new RegExp(/^Fragments\(AAC_und_ch2_128kbps%3d(.*)%2cformat%3dm3u8-aapl\)$/)

  var base = baseAudio
  var reg = audioReg

  fs.readdir(base, (e, files) => {
    files
    .filter( file => file.indexOf("manifest") < 0 )
    .sort( (a, b) => { 
      var pattern = reg
      var aNum = parseInt(a.match(pattern)[1])
      var bNum = parseInt(b.match(pattern)[1])
      return aNum - bNum
    })
    .forEach( (file, index, arr) => {
      console.log("" + index + " - " + file)
      fs.rename(base + file, base + index + '.ts', () => {})
    })
  })
}
```

## Debugging string
Sometimes you may see a weird issue that blows your mind because you never can imagine it happens.
Before blaming your PC, the language or the framework you are working on, it's better to deep dive 
into the underlying data behind the situation.

For example, if you use `console.log` to print out this text `String.fromCharCode(56, 32, 56)`
you will get this: `8 8`. And that's pretty normal. But you'll get the same output if you try 
to print this text `String.fromCharCode(56, 160, 56)`. And if you look at the log, you'll notice that
there is no different between them. But the fact is, the underlying data is completely different.

In those case, to be sure, you should also check the underlying data by representing in some 
eyes-comparable formats such as UTF8 or UTF16. To print UTF16 representation of a text, 
just use the function below:

```js
function strEncodeUTF16(str) {
  var buf = new ArrayBuffer(str.length*2);
  var bufView = new Uint16Array(buf);
  for (var i=0, strLen=str.length; i < strLen; i++) {
    bufView[i] = str.charCodeAt(i);
  }
  return bufView;
}
```

To form a string from UTF16, use this
```js
String.fromCharCode(56, 160, 56)
String.fromCharCode(160)
```

## Read Japanese Text File (Shift_JIS encoded)

```js
import iconv from 'iconv-lite';

async function readFile(path) {
    return new Promise((resolve, reject) => {
        fs.readFile(path, function(err, data) {
            if (err) {
                reject(err);
            }
            var buf = new Buffer.from(data);
            var text = iconv.decode(buf, "Shift_JIS");
            resolve(text);
        });
    });
}
```
