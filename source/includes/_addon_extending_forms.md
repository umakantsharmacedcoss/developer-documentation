### Extending Forms

```php
<?php
// addons/HelloWorldBundle/EventListener/FormSubscriber.php

namespace MauticAddon\HelloWorldBundle\EventListener;

use Mautic\CoreBundle\EventListener\CommonSubscriber;
use Mautic\FormBundle\Event as Events;
use Mautic\FormBundle\FormEvents;

/**
 * Class FormSubscriber
 */
class FormSubscriber extends CommonSubscriber
{

    /**
     * {@inheritdoc}
     */
    static public function getSubscribedEvents()
    {
        return array(
            FormEvents::FORM_ON_BUILD => array('onFormBuilder', 0)
        );
    }

    /**
     * Add a simple email form
     *
     * @param FormBuilderEvent $event
     */
    public function onFormBuilder(Events\FormBuilderEvent $event)
    {
        // Register a form submit actions
        $event->addSubmitAction(
            'helloworld.sendemail',
            array(
                // Label to group by in the dropdown
                'group'       => 'addon.helloworld.header',
                
                // Label to list by in the dropdown
                'label'       => 'addon.helloworld.formaction.send_email',
                'description' => 'addon.helloworld.formaction.send_email_descr',
                
                // Form service for custom config options
                'formType'    => 'helloworld_worlds',
                'formTheme'   => 'HelloWorldBundle:FormTheme\SubmitAction',
                
                // Custom validation callback
                'validator'   => '\MauticAddons\HelloWorldBundle\Helper\FormSubmitHelper::validateForm',
                
                // Callback method to be executed after the submission 
                'callback'    => '\MauticAddons\HelloWorldBundle\Helper\FormSubmitHelper::sendEmail'
            )
        );

        // Register a custom form field
        $event->addFormField(
            'helloworld.customfield',
            array(
                // Field label
                'label'    => 'addon.helloworld.formfield.customfield',
                
                // Form service for the field's configuration
                'formType' => 'helloworld_worlds',
                
                // Template to use to render the formType
                'template' => 'HelloWorldBundle:SubscribedEvents\FormField:customfield.html.php'
            )
        );
    }
}
```

Forms can be extended by listening to the `\Mautic\FormBundle\FormEvents::FORM_ON_BUILD` event.  Read more about [listeners and subscribers](#events). 

#### Form Fields
To add a custom form field, use the `$event->addFormField($identifier, $parameters)` method. `$identifier` must be something unique. The `$parameters` array can contain the following elements:

Key|Required|Type|Description
---|--------|----|-----------
**label**|REQUIRED|string|The language string for the option in the dropdown
**formType**|REQUIRED|string|The alias of a custom form type used to set config options
**formTypeOptions**|OPTIONAL|array|Array of options to include into the formType's $options argument
**formTypeCleanMasks**|OPTIONAL|array|Array of input masks to clean a values from formType
**formTypeTheme**|OPTIONAL|string|Theme to customize elements for formType     
**template**|REQUIRED|string|View template used to render the formType
**valueFilter**|OPTIONAL|mixed|Filter to use to clean the submitted value as supported by InputHelper or a callback function that accepts the arguments `\Mautic\FormBundle\Entity\Field $field` and `$value`.
**valueConstraints**|OPTIONAL|mixed|Callback function to use to validate the value; the function should accept the arguments `\Mautic\FormBundle\Entity\Field $field` and `$filteredValue`.
**builderOptions**|OPTIONAL|array|Array of boolean options for the form builder: <br />addHelpMessage = true/false<br />addShowLabel = true/false<br />addDefaultValue = true/false<br />addLabelAttributes = true/false<br />addInputAttributes = true/false<br />addIsRequired = true/false


#### Form Submit Actions

To add an action, use the `$event->addSubmitAction($identifier, $parameters)` method. `$identifier` must be something unique. The `$parameters` array can contain the following elements:
 
Key|Required|Type|Description
---|--------|----|-----------
**label**|REQUIRED|string|The language string for the option in the dropdown
**description**|OPTIONAL|string|The language string to use for the option's tooltip
**validator**|OPTIONAL|mixed|Static callback function called to validate the form submission
**callback**|REQUIRED|mixed|Static callback function called after a submission (submit action logic goes here)
**formType**|OPTIONAL|string|The alias of a custom form type used to set config options
**formTypeOptions**|OPTIONAL|array|Array of options to include into the formType's $options argument
**formTypeCleanMasks**|OPTIONAL|array|Array of input masks to clean a values from formType
**formTypeTheme**|OPTIONAL|string|Theme to customize elements for formType
**template**|OPTIONAL|string|View template used to render the formType 

The `callback` and `validator` functions can accept the following variables (determined via ReflectionMethod::invokeArgs()):

Variable|Type|Description
--------|----|-----------
**config**|array|Values saved from the custom config options in `formType` 
**post**|array|$_POST
**server**|array|$_SERVER
**factory**|Mautic\CoreBundle\Factory\MauticFactory|[Mautic's factory service](#factory-service)
**fields**|array|Array of the form's fields
**form**|Mautic\FormBundle\Entity\Form|Form entity 
**action**|Mautic\FormBundle\Entity\Action|Action entity 
**submission**|Mautic\FormBundle\Entity\Submission|Submission entity for this submission
