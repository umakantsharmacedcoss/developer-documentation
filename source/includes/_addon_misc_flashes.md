### Flash Messages

```php
<?php

// From within a controller

$this->addFlash(
    'mautic.translation.key',
    array('%placeholder%' => 'some text'),
    'notice', // Notification type
    'flashes', // Translation domain
    $addNotification // Add a notification entry
);
```

```php
<?php

// From within a model or other service with MauticFactory access

$session          = $this->factory->getSession();
$translatedString = $this->factory->getTranslator()->trans(, 
    array(
        '%placeholder%' => 'some text'
    ),
    'flashes'
);
$this->factory->getSession()->getFlashBag()->add('notice', $translatedString);
```

To create an alert, aka flash message, you can use the flash bag in the session.
 
If your controller extends one of [Mautic's common controllers](#controllers), you can simply use the helper function `addFlash()`.

From a model, or any service, you can use the session to obtain the flash bag.

`$flashBag = $this->factory->getSession()->getFlashBag();`

### Notifications

```php
<?php
// From within a controller

$this->addNotification($message, $type, $isRead, $header, $iconClass, new \DateTime());
```

```php
<?php

// From within a model or other service with MauticFactory access

/** @var \Mautic\CoreBundle\Model\NotificationModel $notificationModel */
$notificationModel = $this->factory->getModel('core.notification');
$notificationModel->addNotification($message, $type, $isRead, $header, $iconClass, $datetime );

```
Mautic also has a notification center.  By default, addFlash() will also add a notification to the center. But, a message can be manually added as well.  

Controllers can use the helper function while models and other services can obtain the NotificationModel.
 