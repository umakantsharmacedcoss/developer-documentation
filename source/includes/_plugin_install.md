## Install/Upgrade

```php
<?php
// plugins/HelloWorldBundle/HelloWorldBundle.php

namespace MauticPlugin\HelloWorldBundle;

use Mautic\PluginBundle\Bundle\PluginBundleBase;
use Mautic\PluginBundle\Entity\Plugin;
use Mautic\CoreBundle\Factory\MauticFactory;

class HelloWorldBundle extends PluginBundleBase
{

    /**
     * Called by PluginController::reloadAction when adding a new plugin that's not already installed
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
     * Called by PluginController::reloadAction when the plugin version does not match what's installed
     *
     * @param Plugin         $plugin
     * @param MauticFactory $factory
     */
    static public function onUpdate(Plugin $plugin, MauticFactory $factory)
    {
        // Execute update stuff such as upgrading the database schema
        // $plugin is the Plugin entity with details of what was installed previously
        
        $oldVersion = $plugin->getVersion();
        
        $db     = $factory->getDatabase();
        $schema = $db->getSchemaManager();
                
        // Do stuff with $schema
    }
}
```

Currently, plugins are installed by uploading the plugin to the plugins folder. Then, the admin must click the Install/Upgrade Plugins button. When that happens, new plugins found will have the static method `onInstall()` called from the [plugin's bundle file](#plugin-directory-structure). If the plugin has already been installed, and the version has changed, then `onUpdate()` is called.