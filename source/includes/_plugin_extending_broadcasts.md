### Extending Broadcasts

Broadcasts are communications sent in bulk through a channel such as email (segment emails). Mautic 2.2.0 introduced a new event to execute the sending of these bulk communications via the `mautic:broadcasts:send` command.

```php
<?php
// plugins\HelloWorldBundle\EventListener\BroadcastSubscriber

namespace MauticPlugin\HelloWorldBundle\EventListener;

use Mautic\CoreBundle\CoreEvents;
use Mautic\CoreBundle\Event\ChannelBroadcastEvent;
use MauticPlugin\HelloWorldPlugin\Model\WorldModel;

/**
 * Class BroadcastSubscriber
 */
class BroadcastSubscriber extends CommonSubscriber
{
    /**
     * @var WorldModel
     */
    protected $model;

    /**
     * BroadcastSubscriber constructor.
     *
     * @param WorldModel $model
     */
    public function __construct(WorldModel $model)
    {
        $this->model = $model;
    }

    /**
     * {@inheritdoc}
     */
    public static function getSubscribedEvents()
    {
        return [
            CoreEvents::CHANNEL_BROADCAST => ['onChannelBroadcast', 0]
        ];
    }


    public function onDataCleanup (ChannelBroadcastEvent $event)
    {
        if (!$event->checkContext('world')) {
            return;
        }

        // Get list of published broadcasts or broadcast if there is only a single ID
        $id         = $event->getId();
        $broadcasts = $this->model->getRepository()->getPublishedBroadcasts($id);
        $output     = $event->getOutput();

        while (($broadcast = $broadcasts->next()) !== false) {
            list($sentCount, $failedCount, $ignore) = $this->model->sendIntergalacticMessages($broadcast[0], null, 100, true, $output);
            $event->setResults($this->translator->trans('plugin.helloworld').': '.$broadcast[0]->getName(), $sentCount, $failedCount);
        }
    }
}
```

To hook into the `mautic:broadcasts:send` command, create a listening for the `\Mautic\CoreBundle\CoreEvents::CHANNEL_BROADCAST` event. The event listener should check for the appropriate context and ID. See example code.