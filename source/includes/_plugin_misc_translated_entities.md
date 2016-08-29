### Implementing Translation Support to Entities

Mautic has some helper methods with adding support for translated content to an entity.

#### \Mautic\CoreBundle\Entity\TranslationInterface

This Entity interface ensures that everything is needed in order for Mautic to handle translations correctly for an entity.

#### \Mautic\CoreBundle\Entity\TranslationEntityTrait

This trait provides properties needed to define an Entity's language and relationships to other items. In the Entity's `loadMetadata()` method, be sure to call `$this->addTranslationMetadata()`.

#### \Mautic\CoreBundle\TranslationModelTrait

This trait provides the method `getTranslatedEntity()` that will determine the entity to use as the translation based on the `$lead` and/or the `HTTP_ACCEPT_LANGUAGE` header. It also has a `postTranslationEntitySave()` that should be called at the end of the Entity's `saveEntity()` method. 

#### \Mautic\CoreBundle\Doctrine\TranslationMigrationTrait

To ease the generation of schema to match the Entity, use this trait then execute `$this->addTranslationSchema()`.

#### Translated Entity Form

Add a `locale` and `translatedParent` form fields like the code example. 

```php
<?php 
// plugins/HelloWorldPlugin/Form/Type/WorldType.php

    $transformer = new \Mautic\CoreBundle\Form\Transformer\IdToEntityModelTransformer($this->em, 'HelloWorldBundle:World');
    $builder->add(
        $builder->create(
            'translationParent',
            'world_list',
            array(
                'label'       => 'mautic.core.form.translation_parent',
                'label_attr'  => array('class' => 'control-label'),
                'attr'        => array(
                    'class'   => 'form-control',
                    'tooltip' => 'mautic.core.form.translation_parent.help'
                ),
                'required'    => false,
                'multiple'    => false,
                'empty_value' => 'mautic.core.form.translation_parent.empty',
                'top_level'   => 'translation',
                'ignore_ids'  => array((int) $options['data']->getId())
            )
        )->addModelTransformer($transformer)
    );

    $builder->add(
        'language',
        'locale',
        array(
            'label'      => 'mautic.core.language',
            'label_attr' => array('class' => 'control-label'),
            'attr'       => array(
                'class'   => 'form-control',
                'tooltip' => 'mautic.page.form.language.help',
            ),
            'required'   => false,
            'empty_data' => 'en'
        )
    );
```