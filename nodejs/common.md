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
