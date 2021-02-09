## Using vector format as assets

When using vector format as assets in Xcode, don't forget to check the `Preserve vector data`.
Without this option, your vector image will be converted to png images at build time. 
Therefore, you will lose the benefit of not depeding on resolution of those vector images.
