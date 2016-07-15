### Mail Helper

```php
<?php
$mailer = $this->get('mautic.helper.mailer')->getMailer();

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

use Mautic\EmailBundle\Swiftmailer\Exception\BatchQueueMaxException;

$mailer = $this->get('mautic.helper.mailer')->getMailer();
$failed = array();

foreach ($emailList as $email) {
    try {
        if (!$mailer->addTo($email['email'], $email['name'])) {
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