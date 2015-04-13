## Common Services

### Mautic Factory

Mautic's factory service gives quick access to the most commonly used services such as the router, request, translator, database, etc along with providing a few helper methods.

#### Obtaining the Factory Service

For [controllers](#controllers), extend either `\Mautic\CoreBundle\Controller\CommonController` or `\Mautic\CoreBundle\Controller\FormController` and it will be available via `$this->factory` by default. Otherwise, obtain the factory from the service container via `$factory = $this->container->get('mautic.factory');`
  
For [models](#models), it will be available via `$this->factory` by default.
  
For custom [services](#services), pass 'mautic.factory' as an argument and MauticFactory will be passed into the __construct of the service.

### Translator

```php
<?php 
// Simple string
echo $translator->trans('addon.helloworld.goodbye');

// Simple string with placeholders
echo $translator->trans('addon.helloworld.greeting', array('%name%' => $name));

// String from a domain other than messages (will use planets.ini)
echo $translator->trans('addon.helloworld.worlds', array('%world%' => $world), 'planets');

// Plural translations
$planetCount = 3;
echo $translator->transChoice('addon.helloworld.number_of_planets', $planetCount, array('%planets%' => $planetCount));

// Check to see if a translation key exists
if ($translator->hasId('addon.helloworld.goodbye')) {
    echo $translator->trans('addon.helloworld.goodbye');
} else {
    // other logic
}

// Use the first key if it exists, otherwise use the second (helpful to prevent managing duplicate keys with the same string)
echo $translator->transConditional('addon.helloworld.planets.' . $planet, 'addon.helloworld.dwarf_planets. ' . $planet);
```

Use the translator service to include translated strings in the code. Depending on where the translation is necessary will determine how to obtain the service.

From controllers, models, and any service that has access to [Mautic's factory service](#mautic-factory) (mautic.factory), simply use `$translator = $this->factory->getTranslator();`.
 
To use the template service in view templates, simply use the [template helper](#translation-helper), `$view['translator']`.

The translator service has the following functions to help with translating strings:

**Simple translation**<br />
`trans($id, array $parameters = array(), $domain = null, $locale = null)`

**[Pluralization](http://symfony.com/doc/current/components/translation/usage.html#pluralization)**<br /> 
`transChoice($id, $number, array $parameters = array(), $domain = null, $locale = null)`

**Check to see if a key exists**<br />
`hasId($id, $domain = null, $locale = null)`

**Use the $preferred key if it exists, if not, use $alternative**<br />
`transConditional($preferred, $alternative, $parameters = array(), $domain = null, $locale = null)`

### Router

```php
<?php 

$router = $this->factory->getRouter();

// Relative URL
$url = $router->generateUrl('addon_helloworld_admin');

// URL with placeholders
$url = $router->generateUrl('addon_helloworld_world', array('%world%', 'mars'));

// Absolute URL
$absoluteUrl = $router->generateUrl('addon_helloworld_admin', array(), true);
```

```php
<?php 

// Generate a URL in a view template
$url = $view['router']->generate('addon_helloworld_admin');

```

For controllers, models, and services with [Mautic's factory service](#mautic-factory) set as an argument, the router service can be easily obtained from using `$this->factory->getRouter()`.
 
For views, use the `$view['router']` helper. The difference with the [template helper](#router-helper) is that `generate()` is used instead of `generateUrl`.

### Request

```php
<?php

// $_GET
$get = $this->request->query->all();

// $_POST
$post = $this->request->request->all();

// $_COOKIE
$cookies = $this->request->cookies->all();

// $_SERVER
$server = $this->request->server->all();

// Headers
$headers = $this->request->headers->all();

// Attributes - custom parameters
$headers = $this->request->attributes->all();

// Check if a parameter exists
if ($this->request->request->has('hello') {
    // do something
}

// Retrieve value of a specific parameter setting mars as default
$world = $this->request->query->get('world', 'mars');

// Set custom request value
$this->request->attributes->set('hello', 'world');

// Get the value of a nested array
$mars = $this->request->request->get('world[mars]', array(), true);
```

There are multiple ways to obtain the request service.
 
If the controller is extending one of [Mautic's controllers](#controllers), it is already available via `$this->request`.

Otherwise, if from with a model or a service with access to [Mautic's factory service](#mautic-factory), use `$this->factory->getRequest()`.

From within a view, use `$app->getRequest()`.

### Session

```php
<?php

$session = $this->getSession();

// Get all session parameters
$all = $session->all();

// Get specific parameter setting mars as default
$world = $session->get('helloworld.world', 'mars');

// Check if a parameter exists
if ($session->has('helloworld.world')) {
    // do something
}

// Set a session parameter
$session->set('helloworld.world', 'mars');

// Remove a session parameter
$session->remove('helloworld.world');

// Clear the whole session
$session->clear();
```

To obtain the session service from a controller, model, or a service with access to [Mautic's factory service](#mautic-factory), use `$this->factory->getSession()`.

From within a view, use `$app->getSession()`.