### Paths Helper
```php
<?php 
$pathsHelper = $this->get('mautic.helper.paths');

$relativeImagesDir = $pathsHelper->getSystemPath('images'); // media/images
$absoluteImageDir  = $pathsHelper->getSystemPath('images', true); // /home/user/public_html/media/images
```

* Service name: `mautic.helper.paths`
* Class: `Mautic\CoreBundle\Helper\PathsHelper`

This helper should be used when retrieving system paths for Mautic (images, themes, cache, etc)

There is also a `tmp` or `temporary` option to store temporary files. It should be used by developers for such use case instead of the general `cache` location.  