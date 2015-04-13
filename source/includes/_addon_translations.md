## Translations

Mautic uses INI files for translations.

HelloWorldBundle/<br />
- - - Translations/<br />
- - - - - - en_US/<br />
- - - - - - - - - messages.ini

<aside class="notice">
All addons should include strings for the United States English locale (en_US) as it is the default for the system. 
</aside>

The directory structure for translations should be `Translations/locale/domain.ini`.
 
 
### Domains

Language strings can be organized into domains. Each domain should be its own file in the addon's language locale folder(s). The addon can use any domain it wants but Mautic makes consistent use of three domains:

Domain | Description
-------|------------
messages | Default domain for the translator service when no domain is specified
flashes | Domain for [flash messages](#flash-messages)
validators | Domain for [form validation](#validation) messages

### INI files

> Sample INI files

```
; addons/HelloWorldBundle/Translations/en_US/messages.ini

addon.helloworld.contact_us="Contact Us"
addon.helloworld.goodbye="Goodbye and have a good day!"
addon.helloworld.greeting="Hello %name%!"
addon.helloworld.index="Hello World"
addon.helloworld.manage_worlds="Manage Worlds"
addon.helloworld.number_of_planets="{0}0 planets|{1}1 planet|]1,Inf[%planets% planets"
addon.helloworld.world="World"
addon.helloworld.worlds="%world% Description"
```

```
; addons/HelloWorldBundle/Translations/en_US/flashes.ini

addon.helloworld.notice.thank_you="Thank you %name% for your interest! We will be in contact soon." 
addon.helloworld.notice.planet_demoted="%planet% has been demoted to a dwarf planet." 
addon.helloworld.error.planet_demotion_failed="%planet% could not be demoted because the scientists say so."
```

General guidelines for the translation keys:

1. Segment the key using a period
2. Use underscores to separate words
3. Must be unique
4. Be short yet descriptive
5. Use all lowercase letters and numbers (no punctuation in the key other than period or underscore)

Guidelines for translation strings:

1. Wrap placeholders with %%
2. Use a single key for duplicate translation strings.
3. Use &quot; for double quotes
4. HTML is allowed

### Using the translator
Refer to the [translator service](#translator) to learn how to use translations in the code.