# MauticJS API

Mautic provides a means for plugins to inject custom JavaScript into mtc.js, the PHP generated script that manages Mautic's tracking pixel, dynamic web content, etc. mtc.js is embedded in 3rd party websites to manage communication between those and Mautic.

## mtc.js

```php
<?php
// plugins/HelloWorldPlugin/Event/BuildJsSubscriber.php

namespace MauticPlugin\HelloWorldBundle\EventListener;

use Mautic\CoreBundle\CoreEvents;
use Mautic\CoreBundle\Event\BuildJsEvent;

/**
 * Class BuildJsSubscriber
 */
class BuildJsSubscriber extends CommonSubscriber
{
    /**
     * @return array
     */
    public static function getSubscribedEvents()
    {
        return array(
            CoreEvents::BUILD_MAUTIC_JS => array('onBuildJs', 0)
        );
    }

    /**
     * @param BuildJsEvent $event
     *
     * @return void
     */
    public function onBuildJs(BuildJsEvent $event)
    {
        $js = <<<JS
MauticJS.documentReady(function() {
    // Determine what planet this is coming from
});
JS;
        $event->appendJs($js, 'HelloWorld');
    }
}
```

To inject custom Javascript into mtc.js, use an [event listener](#events) for the `CoreEvents::BUILD_MAUTIC_JS` event. This event receives a `Mautic\CoreBundle\Event\BuildJsEvent` object where `$event->appendJs($js, $sectionName);` can be used to inject the script's code.

<aside class="warning">
Only native Javascript or <a href="#mauticjs-api-functions">MauticJs API functions</a> should be used since jQuery and other libraries are not guaranteed to be available in 3rd party websites.
</aside>
