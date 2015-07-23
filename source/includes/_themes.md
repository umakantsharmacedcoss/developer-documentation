# Themes

Custom themes for public facing areas of Mautic can be generated but require a bit of PHP experience (for now).

The themes use the same templating formats as Symfony's templates and Mautic's view templates. Review the [Views](#views) section for more on using Symfony's PHP templating engine and the `$view` helpers.
 
## Theme Directory Structure
 
 Each theme directory must have at least a config.php file and a html directory with the public facing template's for the feature's it supports. All themes should have a `html/message.html.php` file.  See below for a typical directory structure:
   
  themes/HelloWorld/<br />
  - - - [config.php](#theme-config-file)<br />
  - - - html/ <br />
  - - - - - - [base.html.php](#theme-html-files)<br />
  - - - - - - [email.html.phpp](#theme-html-files)<br />
  - - - - - - [form.html.phpp](#theme-html-files)<br />
  - - - - - - [message.html.phpp](#theme-html-files)<br />
  - - - - - - [page.html.phpp](#theme-html-files)<br />
  
## Theme Config File
 
 ```php
 <?php
// themes/HelloWorld/config.php

$config = array(
    'name'     => 'Hello World',
    'features' => array(
        'page',
        'email',
        'form'
    ),
    'slots'    => array(
        'page'  => array(
            'page_title' => array('type' => 'text', 'placeholder' => 'mautic.page.builder.addcontent'),
            'top_title'  => array('type' => 'text', 'placeholder' => 'mautic.page.builder.addcontent'),
            'top'        => array('type' => 'html', 'placeholder' => 'mautic.page.builder.addcontent'),
            'main'       => array('type' => 'html', 'placeholder' => 'mautic.page.builder.addcontent'),
            'footer'     => array('type' => 'html', 'placeholder' => 'mautic.page.builder.addcontent')
        ),
        'email' => array(
            'header',
            'body',
            'footer'
        )
    )
);

return $config;
```
 The config file defines the name of the theme, the features it supports, and the slots available for page/email builders.
  
 The config file should return an array with the following keys:
 
 Key|Type|Description
 ---|----|-----------
 name|string|Name displayed in the theme dropdowns
 features|array|Array of features the theme supports. Options currently are email, form, and/or page
 slots|array|Array defining slots available for the email and page builders.  Not required for just form themes.
  
## Theme HTML Files

Notice that in the directory structure above, there is a base.html.php file.  This is not necessary but used in the example to define the base HTML document which each some of the following files extend.  Review [Extending Views](#extending-views) for more information.

### email.html.php
```php
<?php 
// themes/HelloBundle/html/email.html.php 
?> 

<!-- Very simple document --> 
<html>
    <head>
        <?php 
        // required for the email builder
        $view['assets']->outputHeadDeclarations(); 
        ?>
    </head>
    <body style="background: #f7f7f7; margin:0">
        <div>
            <?php $view['slots']->output('header'); ?>
        </div>
        <div>
            <?php $view['slots']->output('body'); ?>
        </div>
        <div>
            <?php $view['slots']->output('footer'); ?>
        </div>
        <?php 
        // required for the email builder
        $view['slots']->output('builder'); 
        ?>
    </body>
</html>
```

This file defines the document for building an email template. Of course this file should follow html based email etiquette. Throughout the document should be output for the slots defined in the config file. For example, the config above defines header, body, and footer slots.  Thus in the appropriate places within email.html.php, should be `<?php $view['slots']->output('header'); ?>`, `<?php $view['slots']->output('body'); ?>`,  and `<?php $view['slots']->output('footer'); ?>`,  

<aside class="notice">
Every email.html.php file should have <code>&lt;?php $view['assets']->outputHeadDeclarations();?&gt;</code> in the &lt;head /&gt; tag and <code>&lt;?php $view['slots']->output('builder'); ?&gt;</code> before the closing &lt;/body&gt; tag.  These are used by the email builder and are required.
</aside>

### form.html.php

```php
<?php
// theme/HelloWorld/html/form.html.php

$view->extend(":$template:base.html.php");
?>
<?php if (!empty($message)): ?>
    <div class="well text-center">
        <h2><?php echo $message; ?></h2>
    </div>
<?php endif; ?>

<div class="form-container">
    <?php if (!empty($header)): ?>
        <h4><?php echo ($header); ?></h4>
    <?php endif; ?>
    <?php echo $content; ?>
</div>
```

This file generates the html document for a form when viewed via it's public URL. This does not style the fields of a form. That will be described below.  

Each form.html.php file should echo a `$message`, `$header`, and `$content` variables. `$template` is defined with the name of the current theme used and can be used to extend form.html.php. 

#### Customizing the Form

To provide custom form field templates or to manipulate the form body, create the following directory structure:

  themes/HelloWorld/<br />
  - - - html/ <br />
  - - - - - - MauticFormBundle<br />
  - - - - - - - - - Builder <-- for customizing the form structure itself<br />
  - - - - - - - - - Field <-- for customizing form field types<br />
    

Copy from `app/bundles/FormBundle/Views/Builder/form.html.php` in the theme's Builder directory or one or more of the fields templates in `app/bundles/FormBundle/Views/Field/*.html.php` into the theme's Field directory. Then customize to the desired layout.  

### message.html.php
```php
<?php
// themes/HelloWorld/html/message.html.php

// extend base.html.php which contains the outer HTML document (html, head, and body)
$view->extend(":$template:base.html.php");
?>
<div class="well text-center">
    <h2><?php echo $message; ?></h2>
    <?php if (isset($content)): ?>
    <div class="text-left"><?php echo $content; ?></div>
    <?php endif; ?>
</div>
```

This file is a simple message file mainly used as the landing page for when a lead unsubscribes or resubscribes to the system's emails. But may be used by other areas so should be included in all themes.

It requires echo'ing two variables: `$message` and `$content`. `$message` houses the string message such as "You have been unsubscribed..." `$content` will either be empty or house the HTML of a form that's been associated with the email as an "unsubscribe form." 

### page.html.php

```php
<?php
// themes/HelloBundle/html/page.html.php

// extend base.html.php which contains the outer HTML document (html, head, and body)
$view->extend(":$template:base.html.php");

$parentVariant = $page->getVariantParent();
$title         = (!empty($parentVariant)) ? $parentVariant->getTitle() : $page->getTitle();
$view['slots']->set('pageTitle', $title);
?>

<div class="container">

    <?php if ($view['slots']->hasContent(array('page_title', 'top_title', 'top'))): ?>
    <div class="row">
        <?php if ($view['slots']->hasContent('page_title')): ?>
        <div class="col-lg-12">
            <h1 class="page-header">
                <?php $view['slots']->output('page_title'); ?>
            </h1>
        </div>
        <?php endif; ?>
        <?php if ($view['slots']->hasContent(array('top_title', 'top'))): ?>
        <div class="col-md-12">
            <div class="panel panel-default">
                <?php if ($view['slots']->hasContent('top_title')): ?>
                <div class="panel-heading">
                    <h4><?php $view['slots']->output('top_title'); ?></h4>
                </div>
                <?php endif; ?>
                <?php if ($view['slots']->hasContent('top')): ?>
                <div class="panel-body">
                    <?php $view['slots']->output('top'); ?>
                </div>
                <?php endif; ?>
            </div>
        </div>
        <?php endif; ?>
    </div>
    <?php endif; ?>

    <?php if ($view['slots']->hasContent('main')): ?>
    <div class="row">
       <div class="col-sm-12">
           <?php $view['slots']->output('main'); ?>
       </div>
    </div>
    <?php endif; ?>

    <?php if ($view['slots']->hasContent('footer')): ?>
    <footer>
        <div class="row">
            <div class="col-lg-12">
                <?php $view['slots']->output('footer'); ?>
            </div>
        </div>
    </footer>
    <?php endif; ?>

</div>
<?php $view['slots']->output('builder'); ?>
```

page.html.php is exactly the same as email.html.php except that it'll be used for landing pages instead. Thus, it can be more robust with the HTML document.

The defined slots in the config file can contain properties to tell the builder how to treat the slot.  

<pre class="inline">
    'slots'    => array(
        'page'  => array(
            'page_title' => array('type' => 'text', 'placeholder' => 'mautic.page.builder.addcontent'),
            'top_title'  => array('type' => 'text', 'placeholder' => 'mautic.page.builder.addcontent'),
            'top'        => array('type' => 'html', 'placeholder' => 'mautic.page.builder.addcontent'),
            'main'       => array('type' => 'html', 'placeholder' => 'mautic.page.builder.addcontent'),
            'footer'     => array('type' => 'html', 'placeholder' => 'mautic.page.builder.addcontent')
        ),
</pre>

In the above example, the `page_title` slot only allows text and has a builder placeholder (the text displayed to indicate that it is editable) should be translated using the `mautic.page.builder.addcontent` key (this can be pre-translated text but it's best to use a translatable string for locale support). 

The type option defines how the user can interact with that slot and can be set as text, html, image, or slideshow. 