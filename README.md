# Universal Image Loader for Android

Image loader for Android which can be used for wide cases of asynchronous image displaying.

## Usage

### Simple

``` java
ImageView imageView = ...
String imageUrl = "http://site.com/image.png"; // or "file:///mnt/sdcard/images/image.jpg"

// Get singletone instance of ImageLoader
ImageLoader imageLoader = ImageLoader.getInstance();
// Initialize ImageLoader with configuration. Do it once.
imageLoader.init(ImageLoaderConfiguration.createDefault(context));
// Load and display image asynchronously
imageLoader.displayImage(imageUrl, imageView);
```

### Most detailed
``` java
ImageView imageView = ...
String imageUrl = "http://site.com/image.png"; // or "file:///mnt/sdcard/images/image.jpg"
ProgressBar spinner = ...
File cacheDir = StorageUtils.getCacheDirectory(getApplicationContext(), "UniversalImageLoader/Cache");

// Get singletone instance of ImageLoader
ImageLoader imageLoader = ImageLoader.getInstance();
// Create configuration for ImageLoader
ImageLoaderConfiguration config = new ImageLoaderConfiguration.Builder(getApplicationContext())
			.maxImageWidthForMemoryCache(800)
			.maxImageHeightForMemoryCache(480)
			.httpConnectTimeout(5000)
			.httpReadTimeout(30000)
			.threadPoolSize(5)
			.memoryCache(new UsingFreqLimitedCache(2000000)) // You can pass your own memory cache implementation
			.discCache(new DefaultDiscCache(cacheDir)) // You can pass your own disc cache implementation
			.defaultDisplayImageOptions(DisplayImageOptions.createSimple())
			.build();
// Initialize ImageLoader with created configuration. Do it once.
imageLoader.init(config);

// Creates display image options for custom display task
DisplayImageOptions options = new DisplayImageOptions.Builder()
                                       .showStubImage(R.drawable.stub_image)
                                       .cacheInMemory()
                                       .cacheOnDisc()
                                       .build();
// Load and display image
imageLoader.displayImage(imageUrl, imageView, options, new ImageLoadingListener() {
    @Override
    public void onLoadingStarted() {
       spinner.show();
    }
    @Override
    public void onLoadingComplete() {
        spinner.hide();
    }
});
```


## License
Copyright (c) 2011 [Sergey Tarasevich](http://nostra13android.blogspot.com)

Licensed under the [BSD 3-clause](http://www.opensource.org/licenses/BSD-3-Clause)