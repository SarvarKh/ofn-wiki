This page list things you need to know if you are changing specific parts of the app.

### Image paths
Paths for images and icons should be written like this:
* some form of image helper should always be used, there are various helpers depending on the context and desired output, such as: image_tag, image_path, etc...
* paths should be relative to the /app/assets/images folder
* if an image path starts with a / it will not correctly include the fingerprint, and possibly break
     - for example an image under /app/assets/images/home/banner.jpg should be: image_path("home/banner.jpg")


