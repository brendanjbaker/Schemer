# Database Version Manager (DBVM)
Version 0.1.1

## About

Database Version Manager (dbvm) helps manage database schema versioning. Database versions are represented by sequential integers. Versioning information is stored in a special versioning schema. Source version information is determined by update filenames:

```
1.sql                        # Update 1
2_optional-description.sql   # Update 2
3_.sql                       # Update 3
```

dbvm currently supports recent Postgres versions. Support for other database systems is possible -- submit a pull request!

## Installing dbvm

To install dbvm onto your path, run:

```
cd place/where/you/put/dbvm
./dbvm install
```

To remove it from your path:

```
dbvm uninstall
```

## Configuring dbvm

Most dbvm commands take a configuration file parameter. This configuration path alleviates having to specify the database server, username, versioning schema name, etc., in every operation. You may place a dbvm configuration file anywhere. Its format is:

```
host=127.0.0.1
dbname=postgres
username=john
versioning_schema=widget_versioning
```

DBVM does not handle passwords directly -- if you are using password or MD5 authentication, please ensure your [~/.pgpass](http://www.postgresql.org/docs/current/static/libpq-pgpass.html) file is up-to-date.

## Adding versioning to your project

To begin using dbvm on your database, make certain that the schema specified in your database configuration file exists:

```
psql --command="CREATE SCHEMA widget_versioning AUTHORIZATION john;"
```

Then run:

```
dbvm manager install ~/my-database-configuration-file
```

To remove it, run:

```
dbvm manager uninstall ~/my-database-configuration-file
```

## Applying updates

To update your database schema version to your source schema version, use `dbvm version update`:

```
dbvm version update ~/my-database-configuration-file ~/code/widget-project/database/versions/
```

dbvm will identify the database and source versions it detected, then apply each update necessary:

```
Database version: 0
Source version: 3

Applying update #1 (/d/code/db-test/1_test1-alpha.sql)...
Applying update #2 (/d/code/db-test/2_test2-beta.sql)...
Applying update #3 (/d/code/db-test/3_test3-gamma.sql)...

Up-to-date.
```

If an error is encountered during any update, dbvm exits immediately. Remediate the problem, then issue the update command again to complete the operation.

## Help

Typing `dbvm` or `dbvm <subcommand>` outputs usage and supported commands:

```
usage: dbvm [--version] <command> [<arguments>]

Supported commands are:
install     Install dbvm to /usr/local/bin
manager     Manage management
uninstall   Uninstall dbvm from /usr/local/bin
version     Manage versions
```

