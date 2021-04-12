## NSTextAttachment

You'll get different results with these 2 looks-like-the-same setups below:

Setup 1
```
let attch = NSTextAttachment()
attch.image = image
```

Setup 2
```
let attch = NSTextAttachment(image: image)
```

Here are the result for each setup:

Setup 1: <img width="100" alt="Screen Shot 2021-04-13 at 1 16 01" src="https://user-images.githubusercontent.com/6182631/114427205-d2bfa580-9bf5-11eb-9850-add5bb0ed42c.png">

Setup 2: <img width="100" alt="Screen Shot 2021-04-13 at 1 19 10" src="https://user-images.githubusercontent.com/6182631/114427648-42ce2b80-9bf6-11eb-8f06-64adf3c8b9d4.png">


As you can see, with the setup 1, you'll get the image with original colors.
But with the setup 2, the original colors of the image are not preserved.
