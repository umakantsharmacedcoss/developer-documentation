## Roles and Permissions

Mautic provides a means of defining custom permissions for roles through Permission objects.

### How Permissions Work
Permissions are calculated based on bits assigned to an addon's level and permission.
 
For example, let's say HelloWorldBundle needs to manage access to user's Worlds entity. A permission set for `addon:helloWorld:worlds` might look like

Permission|Bit
----------|---
view|1
edit|2
create|4
delete|8
full|16

<aside class="notice">
<code>addon:helloWorld:worlds:view</code> is a typically notation for requesting permission in Mautic. The notation tells Mautic to check for the `view` permission for the addon, HelloWorldBundle, within the `worlds` level. Levels allow addon's to set permissions for multiple areas.
</aside>

Mautic will take the summation of the bits for the permissions given to a role and store it in the database. For example, if a role is given view and edit access, the stored bit is 3. If given view and create access, the stored bit is 5.

When a permission check is required, e.g. `addon:helloWorld:worlds:create`, Mautic will check if bit of 4 is set in the role's generated bit for `addon:helloWorld:worlds`. If so, permission is granted.

<aside class="notice">
The permission <code>full</code> is reserved to grant access to all previous permissions within the level and thus should always be the highest bit.
</aside>

###Using Permissions

```php
<?php

$security = $this->factory->getSecurity();

// Check if user is granted a single permission
if ($security->isGranted('addon:helloWorld:worlds:view')) {
    // do something
}

// Check if user is granted multiple permissions (must be granted to all to be true)
if ($security->isGranted(
    array(
        'addon:helloWorld:worlds:view',
        'addon:helloWorld:worlds:create',
    )
)
) {
    //do something
}

// Check if user is granted to at least one permission
if ($security->isGranted(
    array(
        'addon:helloWorld:worlds:view',
        'addon:helloWorld:worlds:edit',
    ),
    'MATCH_ONE'
)
) {
    //do something
}

// Get an array of user permissions
$permissions = $security->isGranted(
    array(
        'addon:helloWorld:worlds:view',
        'addon:helloWorld:worlds:edit',
    ),
    'RETURN_ARRAY'
);

if ($permissions['addon:helloWorld:worlds:view']) {
    // do something
}
```

To determine if a user has a specific permission, use Mautic's security service which can be obtained from the [factory service](#factory-service) via `$this->factory->getSecurity()`.

As suggested above, Mautic uses a special permission notation to refer to a specific permission. For core bundles, `bundleName:permissionLevel:permission` is used.  For addons, append `addon:`, i.e. `addon:bundleName:permissionLevel:permission`. `addon:` tells Mautic to look for the permission class from the addons/ directory. 
 
 The...