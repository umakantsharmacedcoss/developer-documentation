## Database
**@todo - refer to [Symfony's](http://symfony.com/doc/current/book/doctrine.html) and [Doctrine's](http://doctrine-orm.readthedocs.org/en/latest) until this section is complete.** 

Mautic uses [Doctrine](#http://doctrine-orm.readthedocs.org/en/latest/), a database object relational mapper (ORM) and database abstraction layer (DBAL) library.
 
Most of Mautic use entity and repository classes to define the schema and interact with the database. 
 
### Metadata
In 1.0.x, Mautic uses [Doctrine annotations driver ](#http://doctrine-orm.readthedocs.org/en/latest/reference/basic-mapping.html) to map entity metadata.

A problem with this method for many shared hosts is the number of cache files it generates (can be 800+ files). Thus in 1.1, metadata mapping will be switched to using the Doctrine's [PHP driver](#http://doctrine-orm.readthedocs.org/en/latest/reference/php-mapping.html#static-function) by leveraging the entity's static `loadMetadata()` function.  