## Database

```php
<?php
// addons/HelloWorldBundle/Entity/World.php

namespace MauticAddon\HelloWorldBundle\Entity;

use Doctrine\ORM\Mapping as ORM;
use Mautic\CategoryBundle\Entity\Category;
use Mautic\CoreBundle\Entity\CommonEntity;

/**
 * Class World
 * @ORM\Table(name="worlds")
 * @ORM\Entity(repositoryClass="MauticAddon\HelloWorldBundle\Entity\WorldRepository")
 */
class World extends CommonEntity
{

    /**
     * @ORM\Column(type="integer")
     * @ORM\Id()
     * @ORM\GeneratedValue(strategy="AUTO")
     */
    private $id;

    /**
     * @ORM\Column(type="string")
     */
    private $name;

    /**
     * @ORM\Column(type="text", nullable=true)
     */
    private $description;

    /**
     * @ORM\ManyToOne(targetEntity="Mautic\CategoryBundle\Entity\Category")
     * @ORM\JoinColumn(onDelete="SET NULL")
     */
    private $category;

    /**
     * @ORM\Column(name="visit_count", type="integer", nullable=true)
     */
    private $visitCount;

    /**
     * @return mixed
     */
    public function getId()
    {
        return $this->id;
    }

    /**
     * @return mixed
     */
    public function getName()
    {
        return $this->name;
    }

    /**
     * @param mixed $name
     *
     * @return World
     */
    public function setName($name)
    {
        $this->name = $name;

        return $this;
    }

    /**
     * @return mixed
     */
    public function getDescription()
    {
        return $this->description;
    }

    /**
     * @param mixed $description
     *
     * @return World
     */
    public function setDescription($description)
    {
        $this->description = $description;

        return $this;
    }

    /**
     * @return Category
     */
    public function getCategory()
    {
        return $this->category;
    }

    /**
     * @param mixed $category
     *
     * @return World
     */
    public function setCategory(Category $category)
    {
        $this->category = $category;

        return $this;
    }

    /**
     * @return mixed
     */
    public function getVisitCount()
    {
        return $this->visitCount;
    }

    /**
     * @param mixed $visitCount
     *
     * @return World
     */
    public function setVisitCount($visitCount)
    {
        $this->visitCount = $visitCount;

        return $this;
    }
    
    /**
     * Increase the visit count by one
     */
    public function upVisitCount()
    {
        $this->visitCount++;
    }
}
```

```php
<?php
// addons/HelloWorldBundle/Entity/WorldRepository.php

namespace MauticAddon\HelloWorldBundle\Entity;

use Mautic\CoreBundle\Entity\CommonRepository;

/**
 * WorldRepository
 */
class WorldRepository extends CommonRepository
{

    public function getEntities($args = array())
    {
        $q = $this
            ->createQueryBuilder('w')
            ->leftJoin('a.category', 'c');

        $args['qb'] = $q;

        return parent::getEntities($args);
    }
}
```

Mautic uses [Doctrine](#http://doctrine-orm.readthedocs.org/en/latest/), a database object relational mapper (ORM) and database abstraction layer (DBAL) library.

Most of Mautic use [entity and repository classes](http://symfony.com/doc/current/book/doctrine.html#creating-an-entity-class) to define the schema and interact with the database.

#### Usage
Refer to [Symfony's](http://symfony.com/doc/current/book/doctrine.html) and [Doctrine's](http://doctrine-orm.readthedocs.org/en/latest) documentation on the specifics of how to interact with the Database.

#### Metadata/Schema
In 1.0.x-1.1.x, Mautic uses [Doctrine annotations driver ](#http://doctrine-orm.readthedocs.org/en/latest/reference/basic-mapping.html) to map entity metadata which in turn defines the schema structure.

<aside class="warning">
A problem with this method for many shared hosts is the number of cache files it generates (can be 800+ files). Thus, planned for 1.2, metadata mapping will be switched to using the Doctrine's <a href="http://doctrine-orm.readthedocs.org/en/latest/reference/php-mapping.html#static-function" target="_new">PHP driver</a> by leveraging the entity's static <code>loadMetadata()</code> function.
</aside>

#### Installing/Updating Schema
When an addon is installed or updated, the bundle's onInstall or onUpgrade functions are called.  These functions can be used to manipulate the database schema. See [Install/Upgrade](#install/upgrade).

#### Table Prefix
Mautic allows custom table prefixes.  If using ORM, there is no need to include the prefix as Mautic will automatically handle it.  However, if there is a need to use Doctrine's DBAL layer directly, the contstant `MAUTIC_TABLE_PREFIX` can be used in conjuction with the table name.

#### Query Builder Best Practices

Due to Mautic supporting both MySQL and Postgres, it is best to use Doctrine's query builders when possible. Here are some notes when using the query builders:

`->groupBy()` must include the same columns included in `->select()`

`->where()` statements that include something like `'my_boolean_column = 1' ` must be
`->where('my_boolean_column = :true')->setParameter('true', true, 'boolean')`

The same applies for `->expr()->eq('l.isPublished', true)`

Instead use

`->where($qb->expr()->eq('l.isPublished', ':true'))->setParameter('true', true, 'boolean')`

Of course the same principle applies to = 0 or false.

`->select(sum(my_boolean_column) as total)` works in MySql but will fail in others.  Either type case the entity property as an integer or use

`count(CASE WHEN my_boolean_column THEN 1 ELSE null END)`

Mysql does not like the above in a `where()` statement.  Use `having()` instead.

Aliases should be lower case or in quotations as Postgres will return the result lower case if not quoted which will lead to PHP errors when used as array keys.

`->select('f.field as myField')` will return as `myfield`

`->select('f.field as "myField"')` will return as `myField`

<strong>Quoting aliases will not work for ORM.  If using ORM, use lower case.</strong>

When using ORM, Mautic will automatically convert DateTime properties to UTC and to the system/user's profile timezone on retrieval.  However, if using DBAL, DateTime strings must be converted to UTC before persisting and to the local timezone on retrieval.  See [Date/Time Helper](#date/time-helper) to assist with conversions.