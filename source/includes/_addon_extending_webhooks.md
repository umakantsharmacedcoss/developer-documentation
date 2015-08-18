## Extending Webhooks

Webhooks allow for Mautic to send data to external services via an endpoint url.

Webhooks work by using event listeners for two main purposes:

1. An event listener to add webhook types to the webhook user interface.
2. An event listener to whichever action you want to trigger a webhook payload to be queued.

##### Additional Steps
Additional steps that you must take in your own bundle are:

1. Create your own event dispatching for your bundle's custom event and payload
2. Register your custom event listeners in your bundle's configuration file as a service.

### Webhook Type Listener
<aside class="notice">
Use the <code>WebhookEvents::WebhookBuilderEvent</code> to add a webhook type to the webhook interface.
</aside>


```php
<?php
namespace Mautic\YourBundle\EventListener;

use Mautic\WebhookBundle\WebhookEvents;
use Mautic\WebhookBundle\Event\WebhookBuilderEvent;
use Mautic\CoreBundle\EventListener\CommonSubscriber;
use Mautic\YourBundle\YourBundleEvents;

/**
 * Class WebhookSubscriber
 *
 * @package Mautic\YourBundle\EventListener
 */

class WebhookSubscriber extends CommonSubscriber
{
    /**
     * @return array
     */
    static public function getSubscribedEvents()
    {
        return array(
            WebhookEvents::WEBHOOK_ON_BUILD => array('onWebhookBuild', 0)
        );
    }

    /**
     * Add event triggers and actions
     *
     * @param WebhookBuilderEvent $event
     */
    public function onWebhookBuild(WebhookBuilderEvent $event)
    {
        // add checkbox to the webhook form for new leads
        $type  = array(
            'label'       => 'mautic.bundlename.webhook.event.type.new',
            'description' => 'mautic.bundlename.webhook.event.type.new_desc', // note: we don't currently use a description, but may in the future so we have supported it here
        );

        // add it to the list
        $event->addEvent(YourBundle::ACTION_TO_TRIGGER, $type);

         // Note: you may create multiple arrays and call the $event->addEvent method multiple times
         // in this function to add several types all at once.
    }
}
?>
```

Add an event listener for the <code>WebhookEvents::WEBHOOK_ON_BUILD</code> event,
call the addEvent method and pass it an argument for your payload event constant,
and an array of a label and a description to be added to the Webhook user interface.

<code>YourBundle::ACTION_TO_TRIGGER</code> is a constant that
should be an event registered in your bundle.
We use the constant to save the type in the database and query for later.
You will use the same constant later, so its important to be consistent.

### Payload Event Listener
<aside class="notice">
    Add an event listener which extends the <code>WebhookSubscriberBase</code> class. This event should be dispatched in your bundle when you wish to create a payload.
</aside>


```php
<?php

namespace Mautic\YourBundle\EventListener;

// its important to use the WebhookSubscriberBase event listener
// as it does a lot of heavy lifting for you.

use Mautic\WebhookBundle\EventListener\WebhookSubscriberBase;

// you should change this to your bundle's event class
use Mautic\LeadBundle\LeadEvents;

// you should change this to the event type that you are going to use.
// in our case it will be a lead event.
use Mautic\LeadBundle\Event\LeadEvent;

use JMS\Serializer\Serializer;
use Mautic\CoreBundle\Factory\MauticFactory;
use Doctrine\ORM\NoResultException;
use Mautic\CoreBundle\EventListener\CommonSubscriber;
use Mautic\LeadBundle\Event\PointsChangeEvent;
use Mautic\ApiBundle\Serializer\Exclusion\PublishDetailsExclusionStrategy;

/**
 * Class LeadSubscriber
 *
 * @package Mautic\Webhook\EventListener
 */
class LeadSubscriber extends WebhookSubscriberBase
{
    /**
     * @return array
     */
    static public function getSubscribedEvents ()
    {
        return array(
            // note this is the same constant we would have used in the addEvent() listener!
            LeadEvents::LEAD_POST_SAVE => array('onLeadNewUpdate', 0)
        )
    }

    /*
     * Generic method to execute when a lead does something
     */
    public function onLeadNewUpdate(LeadEvent $event)
    {
        // serializer groups are defined and optionally used in your bundle's entity.
        // Use these format your payload in JSON formats.
        $serializerGroups = array("leadDetails", "userList", "publishDetails", "ipAddress");

        // the beginning part of building our webhook payload of a lead entity
        $entity = $event->getLead();

        // our payload goes into an array
        $payload = array(
            'lead'      => $entity,
        );

        // we want to only trigger a payload if the lead is new
        if ($event->isNew()) {

            // again please note the use of the constant here - this should be consistent with the constant we register to the webhook form
            $webhookEvents = $this->getEventWebooksByType(LeadEvents::LEAD_POST_SAVE);

            // The webhook model is made available because we've extended the WebhookSubscriberBase class.
            $this->webhookModel->QueueWebhooks($webhookEvents, $payload, $serializerGroups);


        }
    }
}
```

The next step is to add an event listener for the action you want to actually use for creating payloads. This could be anything you want, your bundle will have to have an event type and use the event dispatcher to execute the event.

Here is an example listener to create a lead payload whenever a new lead is added to Mautic.

You can add the listener to any bundle. Be sure to register it in your bundle's config.php file.

<aside class="notice">
    The event constant that you return in the <code>getSubscriberEvents()</code> method should match the event type from the previous listener.
    This is used in a database query, so it must match exactly for the payload to be included in the hook POST to the endpoint URL.
</aside>

You can refer to the model for more documentation on the <code>QueueWebhooks</code> method.
In short you want to pass the event entities that this payload is being queued against
The payload, which is an array, and finally the serializer groups for formatting the JSON.


This should complete the set up registering and executing custom webhook events and payloads.