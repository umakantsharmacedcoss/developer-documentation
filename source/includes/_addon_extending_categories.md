### Extending Categories

Mautic has a CategoryBundle that can be leveraged to incorporate categories into an addon.

#### Configuring Categories for Menu

To add a category menu item for the addon, simply add the following to `menu` [config](#menu) for whichever menu the item should appear (`main` or `admin`):

<pre class="inline">
    'mautic.category.menu.index' => array(
        'bundle' => 'addon:helloWorld'
    )
</pre>

The `bundle` value needs be prefixed with `addon:` as it is used in determining permissions to manage categories. The `helloWorld` should be the bundle name of the addon.

#### Configuring Categories for Routes

There is no need to add custom routes for categories. However, when [generating a URL](#router) to the addon's category list, use

<pre class="inline">
$categoryUrl = $router->generateUrl('mautic_category_index', array('bundle' => 'addon:helloWorld'));
</pre>

#### Including Category in Forms

To add a category select list to a [form](#forms), use `category` as the form type and pass `bundle` as an option:
  
<pre class="inline">
    //add category
    $builder->add('category', 'category', array(
        'bundle' => 'addon:helloWorld'
    ));
</pre>

#### Restricting Category Management

To restrict access to catgories, use the following in the addon's [Permission class](#roles-and-permissions).

In `__construct()` add `$this->addStandardPermissions('categories');` then in `buildForm()`, add `$this->addStandardFormFields('helloWorld', 'categories', $builder, $data);`.

See a code example in [Roles and Permissions](#roles-and-permissions).

The two standard helper methods will add the permissions of `view`, `edit`, `create`, `delete`, `publish`, and `full` for categories.


