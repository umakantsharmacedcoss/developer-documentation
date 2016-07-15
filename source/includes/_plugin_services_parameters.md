### Config Parameters
```php
<?php 
$config = $this->get('mautic.helper.core_parameters');

$apiEnabled = $config->getParameter('helloworld_api_enabled', false);
```

* Service name: `mautic.helper.core_parameters`
* Class: `Mautic\CoreBundle\Helper\CoreParametersHelper`
