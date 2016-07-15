### Cookie Helper

```php
<?php

$cookieHelper = $this->get('mautic.helper.cookie');
$cookieHelper->setCookie('name', 'value', 3600);

$cookieHelper->deleteCookie('name');
```

* Service name: `mautic.helper.cookie`
* Class: `Mautic\CoreBundle\Helper\CookieHelper`

The cookie helper can be used to set cookies based on system settings.