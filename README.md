# Database Version Manager (DBVM)
Version 0.1.1

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

To begin using dbvm on your database, make certain that the schema specified in your database configuration file exists, then run:

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

