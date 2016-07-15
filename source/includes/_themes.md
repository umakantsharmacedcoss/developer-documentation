# Themes

Custom themes for public facing areas of Mautic can be generated but require a bit of Twig experience.

The themes use the same templating formats as [Symfony's twig templates](http://symfony.com/doc/2.8/book/templating.html#templates). 
 
## Theme Directory Structure
 
 Each theme directory must have at least a config.json file and a html directory with the public facing template's for the feature's it supports. All themes should have a `html/message.html.twig` file.  See below for a typical directory structure:
   
  themes/blank/<br />
  - - - [config.json](#theme-config-file)<br />
  - - - [thumbnail.png](#theme-config-file)<br />
  - - - html/ <br />
  - - - - - - [base.html.twig](#theme-html-files)<br />
  - - - - - - [email.html.twig](#theme-html-files)<br />
  - - - - - - [form.html.twig](#theme-html-files)<br />
  - - - - - - [message.html.twig](#theme-html-files)<br />
  - - - - - - [page.html.twig](#theme-html-files)<br />
  
## Theme Config File
 
 ```json
 {
   "name": "Blank",
   "features": [
     "page",
     "email",
     "form"
   ]
 }

```
 The config file defines the name of the theme and the features it supports.
  
 The config file should return an array with the following keys:
 
 Key|Type|Description
 ---|----|-----------
 name|string|Name displayed in the theme dropdowns
 features|array|Array of features the theme supports. Options currently are email, form, and/or page
  
## Slots

### Slot definition

The slot can be defined by a single HTML attribute `data-slot="{slot type here}"`. For example, the text slot can be defined even with the demo content.

When the theme is opened in the builder, the div with attribute `data-slot="text"` will make the text inside the div editable within the inline Froala editor.

The 2 slot types currently built:

Image: Inserts a single image into the div. User can click on it and edit it with options which provides Froala editor (link, change image source, alt text, …)

Button: Insert a HTML button. User can define URL as well as padding, size and position.

### Slot containers

As stated before, users can drag & drop the new slots into the theme. So as a theme developer, you have to define where the user can drop the slots. You can do it again with a single HTML attribute data-slot-container.

```html
<div data-slot-container=””>
<div data-slot=”text”><a>@JaneDoe</a> has invited you to join Awesome inc!</div>
</div>
```

This way the builder will let users drop the new slots into this container. In the example above there is already one predefined slot which user can move to another container, remove or edit.

This functionality will provide you with lots of creative freedom for designing and developing your own unique email and landing pages. Have a unique design? Share it with the community! We would love to see how you’re using Mautic to engage your audience.

## Sections

Sections are full width parts of the theme which can currently (Mautic 2.0.0) let user to change the background color in the section wrapper and in the section itself. Additional functionality like reordering the sections, deleting and adding new are planned.

### Section Wrapper

Section wrapper must have 100% width of the browser window. You thus have to split your theme into several "rows" if you want to enable the users to change the background of each section. The section wrapper can be any block HTML element with attribute `data-section-wrapper`.

### Section

The section itself should be centered and should have fixed width. This fixed width should be consistent with all other sections. Section also wraps the content. The section can be any block HTML element with attribute `data-section`.

## Theme HTML Files

Notice that in the directory structure above, there is a base.html.twig file. This is not necessary but used in the example to define the base HTML document which each some of the following files extend.

### email.html.twig
```twig
{# themes/HelloBundle/html/email.html.twig #} 
<html>
    <head>
    </head>
    <body style="margin:0">
        <div data-section-wrapper>
            <center>
                <table data-section style="width: 600;" width="600" cellpadding="0" cellspacing="0">
                    <tbody>
                        <tr>
                            <td>
                                <div data-slot-container style="min-height: 30px">
                                    <div data-slot="text">
                                        <br>
                                        <h2>Hello there!</h2>
                                        <br>
                                        We haven't heard from you for a while...
                                        <br>
                                        <br>
                                        {unsubscribe_text} | {webview_text}
                                        <br>
                                    </div>
                                </div>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </center>
        </div>
    </body>
</html>

```

This file defines the document for building an email template. Of course this file should follow html based email etiquette. Throughout the document should be output for the slots defined as data attributes.

### form.html.twig

```twig
{# themes/HelloBundle/html/form.html.twig #} 

{% extends ":blank:base.html.twig" %}

{% block content %}
    {% if message is defined %}
        <div>
            <h2>{{ message|raw }}</h2>
        </div>
    {% endif %}

    <div>
        {% if header is defined %}
        <h4>{{ header }}</h4>
        {% endif %}
        {{ content|raw }}
    </div>
{% endblock %}
```

This file generates the html document for a form when viewed via it's public URL. This does not style the fields of a form. That will be described below.  

Each form.html.twig file should output a `message`, `header`, and `content` variables. 

#### Customizing the Form

To provide custom form field templates or to manipulate the form body, create the following directory structure:

  themes/HelloWorld/<br />
  - - - html/ <br />
  - - - - - - MauticFormBundle<br />
  - - - - - - - - - Builder <-- for customizing the form structure itself<br />
  - - - - - - - - - Field <-- for customizing form field types<br />
    
Copy from `app/bundles/FormBundle/Views/Builder/form.html.php` in the theme's Builder directory or one or more of the fields templates in `app/bundles/FormBundle/Views/Field/*.html.php` into the theme's Field directory. Then customize to the desired layout. Note that these must be PHP templates.

### message.html.twig
```twig
{# themes/HelloBundle/html/message.html.twig #}

{% extends ":blank:base.html.twig" %}

{% block content %}
    <div>
        <h2>{{ message|raw }}</h2>
        {% if content is defined %}
        <div>{{ content|raw }}</div>
        {% endif %}
    </div>
{% endblock %}
```

This file is a simple message file mainly used as the landing page for when a lead unsubscribes or resubscribes to the system's emails. But may be used by other areas so should be included in all themes.

It requires echo'ing two variables: `message` and `content`. `message` houses the string message such as "You have been unsubscribed..." `content` will either be empty or house the HTML of a form that's been associated with the email as an "unsubscribe form." 

### page.html.twig

```twig
{# themes/HelloBundle/html/message.html.twig #}
{% extends ":blank:base.html.twig" %}

{% block content %}
<div data-section-wrapper>
    <center>
        <table data-section style="width: 600;" width="600" cellpadding="0" cellspacing="0">
            <tbody>
                <tr>
                    <td>
                        <div data-slot-container style="min-height: 30px">
                            <div data-slot="text">
                                <br>
                                <h2>Hello there!</h2>
                                <br>
                                We haven't heard from you for a while...
                                <br>
                                <br>
                                {unsubscribe_text} | {webview_text}
                                <br>
                            </div>
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
    </center>
</div>
{% endblock %}
```

page.html.twig is exactly the same as email.html.twig except that it'll be used for landing pages instead. Thus, it can be more robust with the HTML document.