### Helpers

Mautic has many helper classes available. A few of the most commonly used ones are highlighted below.

#### Input Helper

The input helper can be used to ensure clean strings from user input.

```php
<?php

use \Mautic\CoreBundle\Helper\InputHelper

// ...

$clean = InputHelper::clean($input);
$clean = InputHelper::int($input);
$clean = InputHelper::alphanum($input);
$clean = InputHelper::html($input);

// and others; refer to the class for more options
```
<div class="clear-right"></div>

#### Cookie Helper

The cookie helper can be used to set cookies based on system settings.

```php
<?php

$cookieHelper = $this->factory->getHelper('cookie');
$cookieHelper->setCookie('name', 'value', 3600);

$cookieHelper->deleteCookie('name');
```
<div class="clear-right"></div>

#### Date/Time Helper

The date/time helper can be used to convert between UTC and the local timezone. 

```php
<?php

$dtHelper  = $this->factory->getDate('2015-07-20 21:39:00', 'Y-m-d H:i:m', 'local');
$utcString = $dtHelper->toUtcString();

// refer to the class for other functions
```
<div class="clear-right"></div>

#### Mail Helper

```php
<?php
$mailer = $this->factory->getMailer();

// To address; can use setTo(), addCc(), setCc(), addBcc(), or setBcc() as well
$mailer->addTo($toAddress, $toName);

// Set a custom from; will use system settings by default
$user = $this->factory->getUser();
$mailer->setFrom(
    $user->getEmail(),
    $user->getFirstName().' '.$user->getLastName()
);

// Set subject
$mailer->setSubject($email['subject']);

// Set content
$mailer->setBody($content);
$mailer->parsePlainText($content);

// Optional lead tracking (array)
$mailer->setLead($lead);
$mailer->setIdHash();

// Send the mail, pass true to dispatch through event listeners (for replacing tokens, etc)
if ($mailer->send(true)) {

    // Optional to create a stat to allow a web view, tracking, etc
    $mailer->createLeadEmailStat();
} else {
    $errors = $mailer->getErrors();
    $failedRecipients = $errors['failures'];
}
```

```php
<?php
// Using queue() for tokenization support

use Mautic\CoreBundle\Swiftmailer\Exception\BatchQueueMaxException;

$mailer = $this->factory->getMailer();
$failed = array();

foreach ($emailList as $email) {
    try {
        if (!$mailer->addTo($email['email'], $email['name']) {
            // Clear the errors so it doesn't stop the next send
            $mailer->clearErrors();

            $failed[] = $email;

            continue;
        }
    } catch (BatchQueueMaxException $e) {
        // Queue full so flush (send) then try again
        if (!$mailer->flushQueue()) {
            $errors = $mailer->getErrors();
            $failed = array_merge($failed, $errors['failures']);
        }

        if (!$mailer->addTo($email['email'], $email['email'], $email['name'])) {
            // ...
        }
    }
}

// Flush pending
if (!$mailer->flushQueue()) {
    // ...
}
```

The mail helper can be used to send email, running the content through the event listeners to search/replace tokens, manipulate the content, etc. 
 
 Some transports, such as Mandrill, support tokenized emails for multiple recipients. The mail helper makes it easy to leverage this feature by using it's `queue()` and `flushQueue()` functions in place of `send()`. If sending a batch of emails, it is recommended to use the `queue()` function.
 
 If using an Email entity (`\Mautic\EmailBundle\Entity\Email`), just pass the Email entity to `$mailer->setEmail($email)` and the subject, body, assets, etc will be extracted and automatically set.
 
#### Attachments
 
 Attachments can be attached to emails by using the `attachFile()` function. You can also attach Mautic assets (`\Mautic\AssetBundle\Entity\Asset`) via `attachAsset()`.
 
 Refer to the class for more details on available functions.