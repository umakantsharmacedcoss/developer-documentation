# Webhooks

Webhook is a universal way how to send data about leads, pages, forms and events. The data are send in real-time when an action occurs so the system which listens form Mautic webhook data can process them without the need for a periodic scanning if Mautic has some new data.

## Available webhook actions

Mautic can send webhook payload when these actions happen:

- Email open
- Form submit
- Lead delete
- Lead point change
- Lead update
- Lead create
- Page hit

## The webhook workflow
 
The example workflow below describes a real-life workflow to get an idea how the webhooks can be used. Let's imagine we have a project management system (PMS) and we want to create a new issue when a lead submits a form.

1. A lead submits a Mautic form.
2. Mautic saves the form.
3. Mautic check if there is some webhook with the *Form submit* event. If there is, send the data to the URL address defined in the webhook.
4. PMS receives the data about the form submission and create a new issue from it.

## Create a webhook

It is possible to create multiple different webhooks. That will allow you to send different information to different apps/scripts.

1. Open the right hand side menu (click the cog icon in the top right corner) and select *Webhooks*.
2. Create a new webhook.
3. Fill in a *Name*, *Webhook POST Url* (see the next paragraph if you don't have one) and select what *Webhook Events* should trigger this webhook.
  
## Test a webhook
 
The easiest way how to test a webhook payload is to use a service like [RequestBin](http://requestb.in/). RequestBin lets you create a URL which you can set as the `Webhook POST Url` in Mautic. Then click the *Apply* button to save it and then click the *Send Test Payload* button. That will send a test payload data to RequestBin and you will be able to see it at your RequestBin. 

When you have your testing webhook created, you can test the real data it sends when a defined action is triggered. 

## Example webhook script

If you need an idea how to receive a Mautic webhook data in your app, this script can be used as a starting point. The script will log the request and return a PHP object of the payload. Place this script to some publicly accessible URL (i.e. `http://yourwebsite.com/webhookTest.php), then fill in the Mautic *Webhook POST Url* to this script. 

 ```php
 <?php
// webhookTest.php

/**
 * A helper class to log and get the Mautic webhook request
 */
class webhookTest {
    /**
     * Log a message to a file
     *
     * @param  mixed $message
     * @param  string $type [info|error|request]
     */
    public function log($message, $type = 'info')
    {
        $prefix = 'webhookLog_';
        $file = $type . '.log';
        $date = new DateTime();
        error_log($date->format('Y-m-d H:i:s') . ' ' . $message . "\n\n", 3, $prefix . $file);
    }
    /**
     * Get the request JSON object and log the request
     *
     * @return object
     */
    public function getRequest()
    {
        $rawData = file_get_contents("php://input");
        $this->log($rawData, 'request');
        return json_decode($rawData);
    }
}
$webhook = new webhookTest;
$requestData = $webhook->getRequest();
// @todo Process the $requestData as needed
```
If you'd like to extend the webhook functionality with your plugin, read more in [the plugin docs](#extending-webhooks).
