### IP Lookup Helper
```php
<?php 
$ipHelper = $this->get('mautic.helper.ip_lookup');

$requestIp = $ipHelper->getIpAddressFromRequest(); // 1.2.3.4

/** @var \Mautic\CoreBundle\Entity\IpAddress $ipAddressEntity */
$ipAddressEntity = $ipHelper->getIpAddress();

/** @var array $details */
$details = $ipAddressEntity->getIpDetails();

echo $details['city'];
```

* Service name: `mautic.helper.ip_lookup`
* Class: `Mautic\CoreBundle\Helper\IpLookupHelper`

This helper can be used to retrieve the real IP for the request.
