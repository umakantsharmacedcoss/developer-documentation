## Install/Upgrade

```php
<?php
// addons/HelloWorldBundle/HelloWorldBundle.php

namespace MauticAddon\HelloWorldBundle;

use Mautic\AddonBundle\Bundle\AddonBundleBase;
use Mautic\AddonBundle\Entity\Addon;
use Mautic\CoreBundle\Factory\MauticFactory;

class HelloWorldBundle extends AddonBundleBase
{

    /**
     * Called by AddonController::reloadAction when adding a new addon that's not already installed
     *
     * @param MauticFactory $factory
     */
    static public function onInstall(MauticFactory $factory)
    {
        // Execute install stuff such as installing database schema
        
        $db     = $factory->getDatabase();
        $schema = $db->getSchemaManager();
        
        // Do stuff with $schema
    }

    /**
     * Called by AddonController::reloadAction when the addon version does not match what's installed
     *
     * @param Addon         $addon
     * @param MauticFactory $factory
     */
    static public function onUpdate(Addon $addon, MauticFactory $factory)
    {
        // Execute update stuff such as upgrading the database schema
        // $addon is the Addon entity with details of what was installed previously
        
        $oldVersion = $addon->getVersion();
        
        $db     = $factory->getDatabase();
        $schema = $db->getSchemaManager();
                
        // Do stuff with $schema
    }
}
```

Currently, addons are installed by uploading the addon to the addons folder. Then, the admin must click the Install/Upgrade Addons button. When that happens, new addons found will have the static method `onInstall()` called from the [addon's bundle file](#addon-directory-structure). If the addon has already been installed, and the version has changed, then `onUpdate()` is called.