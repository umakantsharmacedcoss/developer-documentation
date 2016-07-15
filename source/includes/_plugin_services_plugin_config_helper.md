### Plugin Config Helper
```php
<?php 
$configHelper = $this->get('mautic.helper.bundle');

$menu = $configHelper->getBundleConfig('HelloWorldBundle', 'menu', true);
```

* Service name: `mautic.helper.bundle`
* Class: `Mautic\CoreBundle\Helper\BundleHelper`

This can be used to get the configuration array of a bundle/plugin's Config/config.php file.
