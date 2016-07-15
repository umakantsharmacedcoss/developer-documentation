### User

```php
<?php
$user = $this->get('mautic.helper.user')->getUser();

$firstName = $user->getFirstname();
$lastName  = $user->getLastname();
$email     = $user->getEmail();
$profile   = $user->getProfile();

$role = $user->getRole()->getName();

if ($role->isAdmin()) {
    // do something
}
```

* Service name: `mautic.helper.user`
* Class: `Mautic\CoreBundle\Helper\UserHelper`

`getUser()` will return the [entity](#database), `\Mautic\UserBundle\Entity\User` that can then be used to get information about the currently logged in user.