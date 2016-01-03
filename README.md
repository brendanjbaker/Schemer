# Schemer

## About

Schemer helps manage database schema versioning. Database versions are represented by sequential integers. Versioning information is stored in a special versioning schema. Source versions are determined by filenames:

```
1.sql                        # Update 1
2_optional-description.sql   # Update 2
3_.sql                       # Update 3
```

Schemer currently only supports Postgres. Support for other database systems is possible (pull requests welcomed).

## Installing Schemer

To install `schemer` onto your path, run:

```
git clone git@github.com:brendanjbaker/Schemer.git schemer
cd schemer
./schemer install
```

To remove it from your path:

```
schemer uninstall
```

## Configuring Schemer

Most `schemer` commands take a configuration file parameter. This configuration path alleviates having to specify the database server, username, versioning schema name, etc., in every operation. You may place a Schemer configuration file anywhere. Its format is:

```
host=127.0.0.1
dbname=postgres
username=john
versioning_schema=widget_versioning
```

Schemer does not handle passwords directly. If you are using password or MD5 authentication, please ensure your [~/.pgpass](http://www.postgresql.org/docs/current/static/libpq-pgpass.html) file is up-to-date.

## Adding versioning to your project

To begin using Schemer on your database, make certain that the schema specified in your database configuration file exists:

```
psql --command="CREATE SCHEMA widget_versioning AUTHORIZATION john;"
```

Then run:

```
schemer manager install ~/my-database-configuration-file
```

To remove it, run:

```
schemer manager uninstall ~/my-database-configuration-file
```

## Applying updates

To update your database schema version to your source schema version, use `schemer version update`:

```
schemer version update ~/my-database-configuration-file ~/code/widget-project/database/versions/
```

Schemer will identify the database and source versions it detected, then apply each update necessary:

```
Database version: 0
Source version: 3

Applying update #1 (/d/code/db-test/1_test1-alpha.sql)...
Applying update #2 (/d/code/db-test/2_test2-beta.sql)...
Applying update #3 (/d/code/db-test/3_test3-gamma.sql)...

Up-to-date.
```

If an error is encountered during any update, `schemer` exits immediately. Remediate the problem, then issue the update command again to complete the operation.

## Help

Typing `schemer` or `schemer <subcommand>` outputs usage and supported commands:

```
usage: schemer [--version] <command> [<arguments>]

Supported commands are:
install     Install schemer to /usr/local/bin
manager     Manage management
uninstall   Uninstall schemer from /usr/local/bin
version     Manage versions
```

