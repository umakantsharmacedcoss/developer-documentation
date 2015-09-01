### Events

Mautic leverages Symfony's EventDispatcher to execute and communicate various actions through Mautic. Plugin's can hook into these to extend the functionality of Mautic. Refer to [Extending Mautic](#extending-mautic) for some of the ways to do this.

#### Subscribers

The easiest way to listen to various events is to use an event subscriber. Read more about subscribers in [Symfony's documentation](http://symfony.com/doc/current/components/event_dispatcher/introduction.html#using-event-subscribers). 

Plugin event subscribers should extend `\Mautic\CoreBundle\EventListener\CommonSubscriber` which gives access to MauticFactory and also allows registering the subscriber service through the bundles's config file.  See [Services](#services) for more information on registering event services. 

#### Custom Events

```php
<?php
\\ plugins\HelloWorldBundle\EventListener\LeadSubscriber

namespace MauticPlugin\HelloWorldBundle\EventListener;

use Mautic\LeadBundle\Event as Events;
use Mautic\LeadBundle\LeadEvents;

/**
 * Class LeadSubscriber
 *
 * @package Mautic\LeadBundle\EventListener
 */
class LeadSubscriber extends CommonSubscriber
{

    /**
     * @return array
     */
    static public function getSubscribedEvents()
    {
        return array(
            LeadEvents::LEAD_POST_SAVE     => array('onLeadPostSave', 0),
            LeadEvents::LEAD_POST_DELETE   => array('onLeadDelete', 0)
        );
    }

    public function onLeadPostSave(LeadEvent $event)
    {
        $lead = $event->getLead();
        
        // do something
    }
    
    public function onLeadDelete(LeadEvent $event)
    {
        $lead = $event->getLead();
        
        $deletedId = $lead->deletedId;
        
        // do something
    }
}
```    
    
There are many events available throughout Mautic. Depending on the desired functionality, look at the core bundle's *Event.php file in the root of the bundle.  For example, Lead related events are defined and described in 'app\bundles\LeadBundle\LeadEvents.php'. These final classes provide the names of the events to listen to.  Always use the event constants to ensure future changes to event names will not break the plugin.