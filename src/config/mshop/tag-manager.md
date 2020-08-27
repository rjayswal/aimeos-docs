
# decorators
## excludes

Excludes decorators added by the "common" option from the tag manager

```
mshop/tag/manager/decorators/excludes = Array
(
)
```

* Default: Array
* Type: array - List of decorator names
* Since: 2015.12
* Since: 2015.12

Decorators extend the functionality of a class by adding new aspects
(e.g. log what is currently done), executing the methods of the underlying
class only in certain conditions (e.g. only for logged in users) or
modify what is returned to the caller.

This option allows you to remove a decorator added via
"mshop/common/manager/decorators/default" before they are wrapped
around the tag manager.

```
 mshop/tag/manager/decorators/excludes = array( 'decorator1' )
```

This would remove the decorator named "decorator1" from the list of
common decorators ("\Aimeos\MShop\Common\Manager\Decorator\*") added via
"mshop/common/manager/decorators/default" for the tag manager.

See also:

* mshop/common/manager/decorators/default
* mshop/tag/manager/decorators/global
* mshop/tag/manager/decorators/local
* mshop/common/manager/decorators/default
* mshop/tag/manager/decorators/global
* mshop/tag/manager/decorators/local

## global

Adds a list of globally available decorators only to the tag manager

```
mshop/tag/manager/decorators/global = Array
(
)
```

* Default: Array
* Type: array - List of decorator names
* Since: 2015.12
* Since: 2015.12

Decorators extend the functionality of a class by adding new aspects
(e.g. log what is currently done), executing the methods of the underlying
class only in certain conditions (e.g. only for logged in users) or
modify what is returned to the caller.

This option allows you to wrap global decorators
("\Aimeos\MShop\Common\Manager\Decorator\*") around the tag manager.

```
 mshop/tag/manager/decorators/global = array( 'decorator1' )
```

This would add the decorator named "decorator1" defined by
"\Aimeos\MShop\Common\Manager\Decorator\Decorator1" only to the tag
manager.

See also:

* mshop/common/manager/decorators/default
* mshop/tag/manager/decorators/excludes
* mshop/tag/manager/decorators/local
* mshop/common/manager/decorators/default
* mshop/tag/manager/decorators/excludes
* mshop/tag/manager/decorators/local

## local

Adds a list of local decorators only to the tag manager

```
mshop/tag/manager/decorators/local = Array
(
)
```

* Default: Array
* Type: array - List of decorator names
* Since: 2015.12
* Since: 2015.12

Decorators extend the functionality of a class by adding new aspects
(e.g. log what is currently done), executing the methods of the underlying
class only in certain conditions (e.g. only for logged in users) or
modify what is returned to the caller.

This option allows you to wrap local decorators
("\Aimeos\MShop\Tag\Manager\Decorator\*") around the tag manager.

```
 mshop/tag/manager/decorators/local = array( 'decorator2' )
```

This would add the decorator named "decorator2" defined by
"\Aimeos\MShop\Tag\Manager\Decorator\Decorator2" only to the tag
manager.

See also:

* mshop/common/manager/decorators/default
* mshop/tag/manager/decorators/excludes
* mshop/tag/manager/decorators/global
* mshop/common/manager/decorators/default
* mshop/tag/manager/decorators/excludes
* mshop/tag/manager/decorators/global

# name

Class name of the used tag manager implementation

```
mshop/tag/manager/name = Standard
```

* Default: Standard
* Type: string - Last part of the class name
* Since: 2015.12
* Since: 2015.12

Each default manager can be replace by an alternative imlementation.
To use this implementation, you have to set the last part of the class
name as configuration value so the manager factory knows which class it
has to instantiate.

For example, if the name of the default class is

```
 \Aimeos\MShop\Tag\Manager\Standard
```

and you want to replace it with your own version named

```
 \Aimeos\MShop\Tag\Manager\Mymanager
```

then you have to set the this configuration option:

```
 mshop/tag/manager/name = Mymanager
```

The value is the last part of your own class name and it's case sensitive,
so take care that the configuration value is exactly named like the last
part of the class name.

The allowed characters of the class name are A-Z, a-z and 0-9. No other
characters are possible! You should always start the last part of the class
name with an upper case character and continue only with lower case characters
or numbers. Avoid chamel case names like "MyManager"!


# sitemode

Mode how items from levels below or above in the site tree are handled

```
mshop/tag/manager/sitemode = 3
```

* Default: 3
* Type: int - Constant from Aimeos\MShop\Locale\Manager\Base class
* Since: 2018.01

By default, only items from the current site are fetched from the
storage. If the ai-sites extension is installed, you can create a
tree of sites. Then, this setting allows you to define for the
whole tag domain if items from parent sites are inherited,
sites from child sites are aggregated or both.

Available constants for the site mode are:
* 0 = only items from the current site
* 1 = inherit items from parent sites
* 2 = aggregate items from child sites
* 3 = inherit and aggregate items at the same time

You also need to set the mode in the locale manager
(mshop/locale/manager/standard/sitelevel) to one of the constants.
If you set it to the same value, it will work as described but you
can also use different modes. For example, if inheritance and
aggregation is configured the locale manager but only inheritance
in the domain manager because aggregating items makes no sense in
this domain, then items wil be only inherited. Thus, you have full
control over inheritance and aggregation in each domain.

See also:

* mshop/locale/manager/standard/sitelevel

# standard
## count/ansi

Counts the number of records matched by the given criteria in the database

```
mshop/tag/manager/standard/count/ansi = 
 SELECT COUNT(*) AS "count"
 FROM (
 	SELECT mtag."id"
 	FROM "mshop_tag" AS mtag
 	:joins
 	WHERE :cond
 	ORDER BY mtag."id"
 	OFFSET 0 ROWS FETCH NEXT 10000 ROWS ONLY
 ) AS list
```

* Default: mshop/tag/manager/standard/count
* Type: string - SQL statement for counting items
* Since: 2015.12

Counts all records matched by the given criteria from the tag
database. The records must be from one of the sites that are
configured via the context item. If the current site is part of
a tree of sites, the statement can count all records from the
current site and the complete sub-tree of sites.

As the records can normally be limited by criteria from sub-managers,
their tables must be joined in the SQL context. This is done by
using the "internaldeps" property from the definition of the ID
column of the sub-managers. These internal dependencies specify
the JOIN between the tables and the used columns for joining. The
":joins" placeholder is then replaced by the JOIN strings from
the sub-managers.

To limit the records matched, conditions can be added to the given
criteria object. It can contain comparisons like column names that
must match specific values which can be combined by AND, OR or NOT
operators. The resulting string of SQL conditions replaces the
":cond" placeholder before the statement is sent to the database
server.

Both, the strings for ":joins" and for ":cond" are the same as for
the "search" SQL statement.

Contrary to the "search" statement, it doesn't return any records
but instead the number of records that have been found. As counting
thousands of records can be a long running task, the maximum number
of counted records is limited for performance reasons.

The SQL statement should conform to the ANSI standard to be
compatible with most relational database systems. This also
includes using double quotes for table and column names.

See also:

* mshop/tag/manager/standard/insert/ansi
* mshop/tag/manager/standard/update/ansi
* mshop/tag/manager/standard/newid/ansi
* mshop/tag/manager/standard/delete/ansi
* mshop/tag/manager/standard/search/ansi

## count/mysql

Counts the number of records matched by the given criteria in the database

```
mshop/tag/manager/standard/count/mysql = 
 SELECT COUNT(*) AS "count"
 FROM (
 	SELECT mtag."id"
 	FROM "mshop_tag" AS mtag
 	:joins
 	WHERE :cond
 	ORDER BY mtag."id"
 	LIMIT 10000 OFFSET 0
 ) AS list
```

* Default: 
 SELECT COUNT(*) AS "count"
 FROM (
 	SELECT mtag."id"
 	FROM "mshop_tag" AS mtag
 	:joins
 	WHERE :cond
 	ORDER BY mtag."id"
 	OFFSET 0 ROWS FETCH NEXT 10000 ROWS ONLY
 ) AS list


See also:

* mshop/tag/manager/standard/count/ansi

## delete/ansi

Deletes the items matched by the given IDs from the database

```
mshop/tag/manager/standard/delete/ansi = 
 DELETE FROM "mshop_tag"
 WHERE :cond AND siteid = ?
```

* Default: mshop/tag/manager/standard/delete
* Type: string - SQL statement for deleting items
* Since: 2015.12

Removes the records specified by the given IDs from the tag database.
The records must be from the site that is configured via the
context item.

The ":cond" placeholder is replaced by the name of the ID column and
the given ID or list of IDs while the site ID is bound to the question
mark.

The SQL statement should conform to the ANSI standard to be
compatible with most relational database systems. This also
includes using double quotes for table and column names.

See also:

* mshop/tag/manager/standard/insert/ansi
* mshop/tag/manager/standard/update/ansi
* mshop/tag/manager/standard/newid/ansi
* mshop/tag/manager/standard/search/ansi
* mshop/tag/manager/standard/count/ansi

## delete/mysql

Deletes the items matched by the given IDs from the database

```
mshop/tag/manager/standard/delete/mysql = 
 DELETE FROM "mshop_tag"
 WHERE :cond AND siteid = ?
```

* Default: 
 DELETE FROM "mshop_tag"
 WHERE :cond AND siteid = ?


See also:

* mshop/tag/manager/standard/delete/ansi

## insert/ansi

Inserts a new tag tag record into the database table

```
mshop/tag/manager/standard/insert/ansi = 
 INSERT INTO "mshop_tag" ( :names
 	"langid", "type", "domain", "label",
 	"mtime", "editor", "siteid", "ctime"
 ) VALUES ( :values
 	?, ?, ?, ?, ?, ?, ?, ?
 )
```

* Default: mshop/tag/manager/standard/insert
* Type: string - SQL statement for inserting records
* Since: 2015.12

Items with no ID yet (i.e. the ID is NULL) will be created in
the database and the newly created ID retrieved afterwards
using the "newid" SQL statement.

The SQL statement must be a string suitable for being used as
prepared statement. It must include question marks for binding
the values from the tag tag item to the statement before they are
sent to the database server. The number of question marks must
be the same as the number of columns listed in the INSERT
statement. The order of the columns must correspond to the
order in the saveItems() method, so the correct values are
bound to the columns.

The SQL statement should conform to the ANSI standard to be
compatible with most relational database systems. This also
includes using double quotes for table and column names.

See also:

* mshop/tag/manager/standard/update/ansi
* mshop/tag/manager/standard/newid/ansi
* mshop/tag/manager/standard/delete/ansi
* mshop/tag/manager/standard/search/ansi
* mshop/tag/manager/standard/count/ansi

## insert/mysql

Inserts a new tag tag record into the database table

```
mshop/tag/manager/standard/insert/mysql = 
 INSERT INTO "mshop_tag" ( :names
 	"langid", "type", "domain", "label",
 	"mtime", "editor", "siteid", "ctime"
 ) VALUES ( :values
 	?, ?, ?, ?, ?, ?, ?, ?
 )
```

* Default: 
 INSERT INTO "mshop_tag" ( :names
 	"langid", "type", "domain", "label",
 	"mtime", "editor", "siteid", "ctime"
 ) VALUES ( :values
 	?, ?, ?, ?, ?, ?, ?, ?
 )


See also:

* mshop/tag/manager/standard/insert/ansi

## newid/ansi

Retrieves the ID generated by the database when inserting a new record

```
mshop/tag/manager/standard/newid/ansi = mshop/tag/manager/standard/newid
```

* Default: mshop/tag/manager/standard/newid
* Type: string - SQL statement for retrieving the last inserted record ID
* Since: 2015.12

As soon as a new record is inserted into the database table,
the database server generates a new and unique identifier for
that record. This ID can be used for retrieving, updating and
deleting that specific record from the table again.

For MySQL:
```
 SELECT LAST_INSERT_ID()
For PostgreSQL:
 SELECT currval('seq_mtag_id')
For SQL Server:
 SELECT SCOPE_IDENTITY()
For Oracle:
 SELECT "seq_mtag_id".CURRVAL FROM DUAL
```

There's no way to retrive the new ID by a SQL statements that
fits for most database servers as they implement their own
specific way.

See also:

* mshop/tag/manager/standard/insert/ansi
* mshop/tag/manager/standard/update/ansi
* mshop/tag/manager/standard/delete/ansi
* mshop/tag/manager/standard/search/ansi
* mshop/tag/manager/standard/count/ansi

## newid/mysql

Retrieves the ID generated by the database when inserting a new record

```
mshop/tag/manager/standard/newid/mysql = SELECT LAST_INSERT_ID()
```

* Default: mshop/tag/manager/standard/newid

See also:

* mshop/tag/manager/standard/newid/ansi

## search/ansi

Retrieves the records matched by the given criteria in the database

```
mshop/tag/manager/standard/search/ansi = 
 SELECT :columns
 	mtag."id" AS "tag.id", mtag."siteid" AS "tag.siteid",
 	mtag."type" AS "tag.type", mtag."langid" AS "tag.languageid",
 	mtag."domain" AS "tag.domain", mtag."label" AS "tag.label",
 	mtag."mtime" AS "tag.mtime", mtag."editor" AS "tag.editor",
 	mtag."ctime" AS "tag.ctime"
 FROM "mshop_tag" AS mtag
 :joins
 WHERE :cond
 ORDER BY :order
 OFFSET :start ROWS FETCH NEXT :size ROWS ONLY
```

* Default: mshop/tag/manager/standard/search
* Type: string - SQL statement for searching items
* Since: 2015.12

Fetches the records matched by the given criteria from the tag
database. The records must be from one of the sites that are
configured via the context item. If the current site is part of
a tree of sites, the SELECT statement can retrieve all records
from the current site and the complete sub-tree of sites.

As the records can normally be limited by criteria from sub-managers,
their tables must be joined in the SQL context. This is done by
using the "internaldeps" property from the definition of the ID
column of the sub-managers. These internal dependencies specify
the JOIN between the tables and the used columns for joining. The
":joins" placeholder is then replaced by the JOIN strings from
the sub-managers.

To limit the records matched, conditions can be added to the given
criteria object. It can contain comparisons like column names that
must match specific values which can be combined by AND, OR or NOT
operators. The resulting string of SQL conditions replaces the
":cond" placeholder before the statement is sent to the database
server.

If the records that are retrieved should be ordered by one or more
columns, the generated string of column / sort direction pairs
replaces the ":order" placeholder. In case no ordering is required,
the complete ORDER BY part including the "/*-orderby*/.../*orderby-*/"
markers is removed to speed up retrieving the records. Columns of
sub-managers can also be used for ordering the result set but then
no index can be used.

The number of returned records can be limited and can start at any
number between the begining and the end of the result set. For that
the ":size" and ":start" placeholders are replaced by the
corresponding values from the criteria object. The default values
are 0 for the start and 100 for the size value.

The SQL statement should conform to the ANSI standard to be
compatible with most relational database systems. This also
includes using double quotes for table and column names.

See also:

* mshop/tag/manager/standard/insert/ansi
* mshop/tag/manager/standard/update/ansi
* mshop/tag/manager/standard/newid/ansi
* mshop/tag/manager/standard/delete/ansi
* mshop/tag/manager/standard/count/ansi

## search/mysql

Retrieves the records matched by the given criteria in the database

```
mshop/tag/manager/standard/search/mysql = 
 SELECT :columns
 	mtag."id" AS "tag.id", mtag."siteid" AS "tag.siteid",
 	mtag."type" AS "tag.type", mtag."langid" AS "tag.languageid",
 	mtag."domain" AS "tag.domain", mtag."label" AS "tag.label",
 	mtag."mtime" AS "tag.mtime", mtag."editor" AS "tag.editor",
 	mtag."ctime" AS "tag.ctime"
 FROM "mshop_tag" AS mtag
 :joins
 WHERE :cond
 ORDER BY :order
 LIMIT :size OFFSET :start
```

* Default: 
 SELECT :columns
 	mtag."id" AS "tag.id", mtag."siteid" AS "tag.siteid",
 	mtag."type" AS "tag.type", mtag."langid" AS "tag.languageid",
 	mtag."domain" AS "tag.domain", mtag."label" AS "tag.label",
 	mtag."mtime" AS "tag.mtime", mtag."editor" AS "tag.editor",
 	mtag."ctime" AS "tag.ctime"
 FROM "mshop_tag" AS mtag
 :joins
 WHERE :cond
 ORDER BY :order
 OFFSET :start ROWS FETCH NEXT :size ROWS ONLY


See also:

* mshop/tag/manager/standard/search/ansi

## update/ansi

Updates an existing tag tag record in the database

```
mshop/tag/manager/standard/update/ansi = 
 UPDATE "mshop_tag"
 SET :names
 	"langid" = ?, "type" = ?, "domain" = ?, "label" = ?, "mtime" = ?, "editor" = ?
 WHERE "siteid" = ? AND "id" = ?
```

* Default: mshop/tag/manager/standard/update
* Type: string - SQL statement for updating records
* Since: 2015.12

Items which already have an ID (i.e. the ID is not NULL) will
be updated in the database.

The SQL statement must be a string suitable for being used as
prepared statement. It must include question marks for binding
the values from the tag tag item to the statement before they are
sent to the database server. The order of the columns must
correspond to the order in the saveItems() method, so the
correct values are bound to the columns.

The SQL statement should conform to the ANSI standard to be
compatible with most relational database systems. This also
includes using double quotes for table and column names.

See also:

* mshop/tag/manager/standard/insert/ansi
* mshop/tag/manager/standard/newid/ansi
* mshop/tag/manager/standard/delete/ansi
* mshop/tag/manager/standard/search/ansi
* mshop/tag/manager/standard/count/ansi

## update/mysql

Updates an existing tag tag record in the database

```
mshop/tag/manager/standard/update/mysql = 
 UPDATE "mshop_tag"
 SET :names
 	"langid" = ?, "type" = ?, "domain" = ?, "label" = ?, "mtime" = ?, "editor" = ?
 WHERE "siteid" = ? AND "id" = ?
```

* Default: 
 UPDATE "mshop_tag"
 SET :names
 	"langid" = ?, "type" = ?, "domain" = ?, "label" = ?, "mtime" = ?, "editor" = ?
 WHERE "siteid" = ? AND "id" = ?


See also:

* mshop/tag/manager/standard/update/ansi

# submanagers

List of manager names that can be instantiated by the tag tag manager

```
mshop/tag/manager/submanagers = Array
(
)
```

* Default: Array
* Type: array - List of sub-manager names
* Since: 2015.12

Managers provide a generic interface to the underlying storage.
Each manager has or can have sub-managers caring about particular
aspects. Each of these sub-managers can be instantiated by its
parent manager using the getSubManager() method.

The search keys from sub-managers can be normally used in the
manager as well. It allows you to search for items of the manager
using the search keys of the sub-managers to further limit the
retrieved list of items.


# type
## decorators/excludes

Excludes decorators added by the "common" option from the tag type manager

```
mshop/tag/manager/type/decorators/excludes = Array
(
)
```

* Default: Array
* Type: array - List of decorator names
* Since: 2015.12

Decorators extend the functionality of a class by adding new aspects
(e.g. log what is currently done), executing the methods of the underlying
class only in certain conditions (e.g. only for logged in users) or
modify what is returned to the caller.

This option allows you to remove a decorator added via
"mshop/common/manager/decorators/default" before they are wrapped
around the tag type manager.

```
 mshop/tag/manager/type/decorators/excludes = array( 'decorator1' )
```

This would remove the decorator named "decorator1" from the list of
common decorators ("\Aimeos\MShop\Common\Manager\Decorator\*") added via
"mshop/common/manager/decorators/default" for the tag type manager.

See also:

* mshop/common/manager/decorators/default
* mshop/tag/manager/type/decorators/global
* mshop/tag/manager/type/decorators/local

## decorators/global

Adds a list of globally available decorators only to the tag type manager

```
mshop/tag/manager/type/decorators/global = Array
(
)
```

* Default: Array
* Type: array - List of decorator names
* Since: 2015.12

Decorators extend the functionality of a class by adding new aspects
(e.g. log what is currently done), executing the methods of the underlying
class only in certain conditions (e.g. only for logged in users) or
modify what is returned to the caller.

This option allows you to wrap global decorators
("\Aimeos\MShop\Common\Manager\Decorator\*") around the tag type manager.

```
 mshop/tag/manager/type/decorators/global = array( 'decorator1' )
```

This would add the decorator named "decorator1" defined by
"\Aimeos\MShop\Common\Manager\Decorator\Decorator1" only to the tag
type manager.

See also:

* mshop/common/manager/decorators/default
* mshop/tag/manager/type/decorators/excludes
* mshop/tag/manager/type/decorators/local

## decorators/local

Adds a list of local decorators only to the tag type manager

```
mshop/tag/manager/type/decorators/local = Array
(
)
```

* Default: Array
* Type: array - List of decorator names
* Since: 2015.12

Decorators extend the functionality of a class by adding new aspects
(e.g. log what is currently done), executing the methods of the underlying
class only in certain conditions (e.g. only for logged in users) or
modify what is returned to the caller.

This option allows you to wrap local decorators
("\Aimeos\MShop\Tag\Manager\Type\Decorator\*") around the tag type
manager.

```
 mshop/tag/manager/type/decorators/local = array( 'decorator2' )
```

This would add the decorator named "decorator2" defined by
"\Aimeos\MShop\Tag\Manager\Type\Decorator\Decorator2" only to the tag
type manager.

See also:

* mshop/common/manager/decorators/default
* mshop/tag/manager/type/decorators/excludes
* mshop/tag/manager/type/decorators/global

## name

Class name of the used tag type manager implementation

```
mshop/tag/manager/type/name = Standard
```

* Default: Standard
* Type: string - Last part of the class name
* Since: 2015.12

Each default tag type manager can be replaced by an alternative imlementation.
To use this implementation, you have to set the last part of the class
name as configuration value so the manager factory knows which class it
has to instantiate.

For example, if the name of the default class is

```
 \Aimeos\MShop\Tag\Manager\Type\Standard
```

and you want to replace it with your own version named

```
 \Aimeos\MShop\Tag\Manager\Type\Mytype
```

then you have to set the this configuration option:

```
 mshop/tag/manager/type/name = Mytype
```

The value is the last part of your own class name and it's case sensitive,
so take care that the configuration value is exactly named like the last
part of the class name.

The allowed characters of the class name are A-Z, a-z and 0-9. No other
characters are possible! You should always start the last part of the class
name with an upper case character and continue only with lower case characters
or numbers. Avoid chamel case names like "MyType"!


## standard/count/ansi

Counts the number of records matched by the given criteria in the database

```
mshop/tag/manager/type/standard/count/ansi = 
 SELECT COUNT(*) AS "count"
 FROM (
 	SELECT mtagty."id"
 	FROM "mshop_tag_type" mtagty
 	:joins
 	WHERE :cond
 	ORDER BY mtagty."id"
 	OFFSET 0 ROWS FETCH NEXT 10000 ROWS ONLY
 ) AS list
```

* Default: mshop/tag/manager/type/standard/count
* Type: string - SQL statement for counting items
* Since: 2015.12

Counts all records matched by the given criteria from the product
database. The records must be from one of the sites that are
configured via the context item. If the current site is part of
a tree of sites, the statement can count all records from the
current site and the complete sub-tree of sites.

As the records can normally be limited by criteria from sub-managers,
their tables must be joined in the SQL context. This is done by
using the "internaldeps" property from the definition of the ID
column of the sub-managers. These internal dependencies specify
the JOIN between the tables and the used columns for joining. The
":joins" placeholder is then replaced by the JOIN strings from
the sub-managers.

To limit the records matched, conditions can be added to the given
criteria object. It can contain comparisons like column names that
must match specific values which can be combined by AND, OR or NOT
operators. The resulting string of SQL conditions replaces the
":cond" placeholder before the statement is sent to the database
server.

Both, the strings for ":joins" and for ":cond" are the same as for
the "search" SQL statement.

Contrary to the "search" statement, it doesn't return any records
but instead the number of records that have been found. As counting
thousands of records can be a long running task, the maximum number
of counted records is limited for performance reasons.

The SQL statement should conform to the ANSI standard to be
compatible with most relational database systems. This also
includes using double quotes for table and column names.

See also:

* mshop/tag/manager/type/standard/insert/ansi
* mshop/tag/manager/type/standard/update/ansi
* mshop/tag/manager/type/standard/newid/ansi
* mshop/tag/manager/type/standard/delete/ansi
* mshop/tag/manager/type/standard/search/ansi

## standard/count/mysql

Counts the number of records matched by the given criteria in the database

```
mshop/tag/manager/type/standard/count/mysql = 
 SELECT COUNT(*) AS "count"
 FROM (
 	SELECT mtagty."id"
 	FROM "mshop_tag_type" mtagty
 	:joins
 	WHERE :cond
 	ORDER BY mtagty."id"
 	LIMIT 10000 OFFSET 0
 ) AS list
```

* Default: 
 SELECT COUNT(*) AS "count"
 FROM (
 	SELECT mtagty."id"
 	FROM "mshop_tag_type" mtagty
 	:joins
 	WHERE :cond
 	ORDER BY mtagty."id"
 	OFFSET 0 ROWS FETCH NEXT 10000 ROWS ONLY
 ) AS list


See also:

* mshop/tag/manager/type/standard/count/ansi

## standard/delete/ansi

Deletes the items matched by the given IDs from the database

```
mshop/tag/manager/type/standard/delete/ansi = 
 DELETE FROM "mshop_tag_type"
 WHERE :cond AND siteid = ?
```

* Default: mshop/tag/manager/type/standard/delete
* Type: string - SQL statement for deleting items
* Since: 2015.12

Removes the records specified by the given IDs from the product database.
The records must be from the site that is configured via the
context item.

The ":cond" placeholder is replaced by the name of the ID column and
the given ID or list of IDs while the site ID is bound to the question
mark.

The SQL statement should conform to the ANSI standard to be
compatible with most relational database systems. This also
includes using double quotes for table and column names.

See also:

* mshop/tag/manager/type/standard/insert/ansi
* mshop/tag/manager/type/standard/update/ansi
* mshop/tag/manager/type/standard/newid/ansi
* mshop/tag/manager/type/standard/search/ansi
* mshop/tag/manager/type/standard/count/ansi

## standard/delete/mysql

Deletes the items matched by the given IDs from the database

```
mshop/tag/manager/type/standard/delete/mysql = 
 DELETE FROM "mshop_tag_type"
 WHERE :cond AND siteid = ?
```

* Default: 
 DELETE FROM "mshop_tag_type"
 WHERE :cond AND siteid = ?


See also:

* mshop/tag/manager/type/standard/delete/ansi

## standard/insert/ansi

Inserts a new tag type record into the database table

```
mshop/tag/manager/type/standard/insert/ansi = 
 INSERT INTO "mshop_tag_type" ( :names
 	"code", "domain", "label", "pos", "status",
 	"mtime", "editor", "siteid", "ctime"
 ) VALUES ( :values
 	?, ?, ?, ?, ?, ?, ?, ?, ?
 )
```

* Default: mshop/tag/manager/type/standard/insert
* Type: string - SQL statement for inserting records
* Since: 2015.12

Items with no ID yet (i.e. the ID is NULL) will be created in
the database and the newly created ID retrieved afterwards
using the "newid" SQL statement.

The SQL statement must be a string suitable for being used as
prepared statement. It must include question marks for binding
the values from the product type item to the statement before they are
sent to the database server. The number of question marks must
be the same as the number of columns listed in the INSERT
statement. The order of the columns must correspond to the
order in the saveItems() method, so the correct values are
bound to the columns.

The SQL statement should conform to the ANSI standard to be
compatible with most relational database systems. This also
includes using double quotes for table and column names.

See also:

* mshop/tag/manager/type/standard/update/ansi
* mshop/tag/manager/type/standard/newid/ansi
* mshop/tag/manager/type/standard/delete/ansi
* mshop/tag/manager/type/standard/search/ansi
* mshop/tag/manager/type/standard/count/ansi

## standard/insert/mysql

Inserts a new tag type record into the database table

```
mshop/tag/manager/type/standard/insert/mysql = 
 INSERT INTO "mshop_tag_type" ( :names
 	"code", "domain", "label", "pos", "status",
 	"mtime", "editor", "siteid", "ctime"
 ) VALUES ( :values
 	?, ?, ?, ?, ?, ?, ?, ?, ?
 )
```

* Default: 
 INSERT INTO "mshop_tag_type" ( :names
 	"code", "domain", "label", "pos", "status",
 	"mtime", "editor", "siteid", "ctime"
 ) VALUES ( :values
 	?, ?, ?, ?, ?, ?, ?, ?, ?
 )


See also:

* mshop/tag/manager/type/standard/insert/ansi

## standard/newid/ansi

Retrieves the ID generated by the database when inserting a new record

```
mshop/tag/manager/type/standard/newid/ansi = mshop/tag/manager/type/standard/newid
```

* Default: mshop/tag/manager/type/standard/newid
* Type: string - SQL statement for retrieving the last inserted record ID
* Since: 2015.12

As soon as a new record is inserted into the database table,
the database server generates a new and unique identifier for
that record. This ID can be used for retrieving, updating and
deleting that specific record from the table again.

For MySQL:
```
 SELECT LAST_INSERT_ID()
For PostgreSQL:
 SELECT currval('seq_mtagty_id')
For SQL Server:
 SELECT SCOPE_IDENTITY()
For Oracle:
 SELECT "seq_mtagty_id".CURRVAL FROM DUAL
```

There's no way to retrive the new ID by a SQL statements that
fits for most database servers as they implement their own
specific way.

See also:

* mshop/tag/manager/type/standard/insert/ansi
* mshop/tag/manager/type/standard/update/ansi
* mshop/tag/manager/type/standard/delete/ansi
* mshop/tag/manager/type/standard/search/ansi
* mshop/tag/manager/type/standard/count/ansi

## standard/newid/mysql

Retrieves the ID generated by the database when inserting a new record

```
mshop/tag/manager/type/standard/newid/mysql = SELECT LAST_INSERT_ID()
```

* Default: mshop/tag/manager/type/standard/newid

See also:

* mshop/tag/manager/type/standard/newid/ansi

## standard/search/ansi

Retrieves the records matched by the given criteria in the database

```
mshop/tag/manager/type/standard/search/ansi = 
 SELECT :columns
 	mtagty."id" AS "tag.type.id", mtagty."siteid" AS "tag.type.siteid",
 	mtagty."code" AS "tag.type.code", mtagty."domain" AS "tag.type.domain",
 	mtagty."label" AS "tag.type.label", mtagty."status" AS "tag.type.status",
 	mtagty."mtime" AS "tag.type.mtime", mtagty."editor" AS "tag.type.editor",
 	mtagty."ctime" AS "tag.type.ctime", mtagty."pos" AS "tag.type.position"
 FROM "mshop_tag_type" mtagty
 :joins
 WHERE :cond
 ORDER BY :order
 OFFSET :start ROWS FETCH NEXT :size ROWS ONLY
```

* Default: mshop/tag/manager/type/standard/search
* Type: string - SQL statement for searching items
* Since: 2015.12

Fetches the records matched by the given criteria from the product
database. The records must be from one of the sites that are
configured via the context item. If the current site is part of
a tree of sites, the SELECT statement can retrieve all records
from the current site and the complete sub-tree of sites.

As the records can normally be limited by criteria from sub-managers,
their tables must be joined in the SQL context. This is done by
using the "internaldeps" property from the definition of the ID
column of the sub-managers. These internal dependencies specify
the JOIN between the tables and the used columns for joining. The
":joins" placeholder is then replaced by the JOIN strings from
the sub-managers.

To limit the records matched, conditions can be added to the given
criteria object. It can contain comparisons like column names that
must match specific values which can be combined by AND, OR or NOT
operators. The resulting string of SQL conditions replaces the
":cond" placeholder before the statement is sent to the database
server.

If the records that are retrieved should be ordered by one or more
columns, the generated string of column / sort direction pairs
replaces the ":order" placeholder. In case no ordering is required,
the complete ORDER BY part including the "/*-orderby*/.../*orderby-*/"
markers is removed to speed up retrieving the records. Columns of
sub-managers can also be used for ordering the result set but then
no index can be used.

The number of returned records can be limited and can start at any
number between the begining and the end of the result set. For that
the ":size" and ":start" placeholders are replaced by the
corresponding values from the criteria object. The default values
are 0 for the start and 100 for the size value.

The SQL statement should conform to the ANSI standard to be
compatible with most relational database systems. This also
includes using double quotes for table and column names.

See also:

* mshop/tag/manager/type/standard/insert/ansi
* mshop/tag/manager/type/standard/update/ansi
* mshop/tag/manager/type/standard/newid/ansi
* mshop/tag/manager/type/standard/delete/ansi
* mshop/tag/manager/type/standard/count/ansi

## standard/search/mysql

Retrieves the records matched by the given criteria in the database

```
mshop/tag/manager/type/standard/search/mysql = 
 SELECT :columns
 	mtagty."id" AS "tag.type.id", mtagty."siteid" AS "tag.type.siteid",
 	mtagty."code" AS "tag.type.code", mtagty."domain" AS "tag.type.domain",
 	mtagty."label" AS "tag.type.label", mtagty."status" AS "tag.type.status",
 	mtagty."mtime" AS "tag.type.mtime", mtagty."editor" AS "tag.type.editor",
 	mtagty."ctime" AS "tag.type.ctime", mtagty."pos" AS "tag.type.position"
 FROM "mshop_tag_type" mtagty
 :joins
 WHERE :cond
 ORDER BY :order
 LIMIT :size OFFSET :start
```

* Default: 
 SELECT :columns
 	mtagty."id" AS "tag.type.id", mtagty."siteid" AS "tag.type.siteid",
 	mtagty."code" AS "tag.type.code", mtagty."domain" AS "tag.type.domain",
 	mtagty."label" AS "tag.type.label", mtagty."status" AS "tag.type.status",
 	mtagty."mtime" AS "tag.type.mtime", mtagty."editor" AS "tag.type.editor",
 	mtagty."ctime" AS "tag.type.ctime", mtagty."pos" AS "tag.type.position"
 FROM "mshop_tag_type" mtagty
 :joins
 WHERE :cond
 ORDER BY :order
 OFFSET :start ROWS FETCH NEXT :size ROWS ONLY


See also:

* mshop/tag/manager/type/standard/search/ansi

## standard/update/ansi

Updates an existing tag type record in the database

```
mshop/tag/manager/type/standard/update/ansi = 
 UPDATE "mshop_tag_type"
 SET :names
 	"code" = ?, "domain" = ?, "label" = ?, "pos" = ?,
 	"status" = ?, "mtime" = ?, "editor" = ?
 WHERE "siteid" = ? AND "id" = ?
```

* Default: mshop/tag/manager/type/standard/update
* Type: string - SQL statement for updating records
* Since: 2015.12

Items which already have an ID (i.e. the ID is not NULL) will
be updated in the database.

The SQL statement must be a string suitable for being used as
prepared statement. It must include question marks for binding
the values from the product type item to the statement before they are
sent to the database server. The order of the columns must
correspond to the order in the saveItems() method, so the
correct values are bound to the columns.

The SQL statement should conform to the ANSI standard to be
compatible with most relational database systems. This also
includes using double quotes for table and column names.

See also:

* mshop/tag/manager/type/standard/insert/ansi
* mshop/tag/manager/type/standard/newid/ansi
* mshop/tag/manager/type/standard/delete/ansi
* mshop/tag/manager/type/standard/search/ansi
* mshop/tag/manager/type/standard/count/ansi

## standard/update/mysql

Updates an existing tag type record in the database

```
mshop/tag/manager/type/standard/update/mysql = 
 UPDATE "mshop_tag_type"
 SET :names
 	"code" = ?, "domain" = ?, "label" = ?, "pos" = ?,
 	"status" = ?, "mtime" = ?, "editor" = ?
 WHERE "siteid" = ? AND "id" = ?
```

* Default: 
 UPDATE "mshop_tag_type"
 SET :names
 	"code" = ?, "domain" = ?, "label" = ?, "pos" = ?,
 	"status" = ?, "mtime" = ?, "editor" = ?
 WHERE "siteid" = ? AND "id" = ?


See also:

* mshop/tag/manager/type/standard/update/ansi

## submanagers

List of manager names that can be instantiated by the tag type manager

```
mshop/tag/manager/type/submanagers = Array
(
)
```

* Default: Array
* Type: array - List of sub-manager names
* Since: 2015.12

Managers provide a generic interface to the underlying storage.
Each manager has or can have sub-managers caring about particular
aspects. Each of these sub-managers can be instantiated by its
parent manager using the getSubManager() method.

The search keys from sub-managers can be normally used in the
manager as well. It allows you to search for items of the manager
using the search keys of the sub-managers to further limit the
retrieved list of items.