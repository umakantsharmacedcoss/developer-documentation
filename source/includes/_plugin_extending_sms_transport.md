### Extending SMS Transport

Custom SMS transport can be registered in Mautic by registering a service with a **mautic.sms_transport** service tag. Any service tagged will be considered a SMS Transport service.

#### Service definition

Service must implement translatable alias, in this case: ```mautic.sms.transport.twilio```. 
Service must have service tag **mautic.sms_transport**

```php
'mautic.sms.transport.twilio' => [
        'class'     => 'Mautic\SmsBundle\Api\TwilioApi',
        'arguments' => [
            'mautic.page.model.trackable',
            'mautic.helper.phone_number',
            'mautic.helper.integration',
            'monolog.logger.mautic',
        ],
        'alias' => 'mautic.sms.transport.twilio',
        'tag'  => 'mautic.sms_transport',
        ],
    ],
```


#### Transport Service class

The Transport service must implement **Mautic\SmsBundle\Api\AbstractSmsApi** inteface.

```php
 /**
     * @param string $number
     * @param string $content
     *
     * @return bool|string
     */
    public function sendSms($number, $content)
    {
        try {
            $this->client->account->messages->sendMessage(
                $this->sendingPhoneNumber,
                $this->sanitizeNumber($number),
                $content
            );

            return true;
        } catch (\Services_Twilio_RestException $e) {
            $this->logger->addWarning($e->getMessage(), ['exception' => $e]);

            return $e->getMessage();
        }
    }
```

#### Return values

sendSMS method should return either **true** if message was sent successfuly. If sending fails for any reason a string containing error description should be returned.

#### Transport service selection

When new transport is registered; it will be displayed and can be selected in the configuration section of Mautic UI.
