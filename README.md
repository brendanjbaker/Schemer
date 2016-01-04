# Schemer

## About

Schemer helps manage database schema versioning. Database versions are represented by sequential integers. Versioning information is stored in a special versioning schema. Source versions are determined by filenames:

```
1.sql                        # Update 1
2_optional-description.sql   # Update 2
3_.sql                       # Update 3
```

Schemer currently supports Postgres and MySQL. Support for other database systems is also possible (pull requests welcomed).

## Installing Schemer

To install `schemer` onto your path, run:

```
git clone https://github.com/brendanjbaker/Schemer.git schemer
cd schemer
./install
```

To remove it from your path:

```
cd place/where/you/put/schemer
./uninstall
```

## Configuring Schemer

Most `schemer` commands take a configuration file parameter. This configuration path alleviates having to specify the database server, username, versioning schema name, etc., in every operation. You may place a Schemer configuration file anywhere. Its format is:

```
# Hostname or IP address of your database server
server=127.0.0.1

# Database server type: either "mysql" or "postgres"
server_type=postgres

# Database server username.
username=john

# Postgres-specific: which database to use. Omit if using MySQL.
postgres_database=postgres
```

Schemer does not handle passwords directly. If you are using password or MD5 authentication, please ensure your [~/.pgpass](http://www.postgresql.org/docs/current/static/libpq-pgpass.html) file is up-to-date.

## Adding versioning to your project

To begin using Schemer on your `widget_gallery` database, use `schemer create`:

```
schemer create widget_gallery
```

## Applying updates

To update your database schema version to your source schema version, use `schemer update`:

```
schemer update ~/my-database-configuration-file ~/code/widget-project/database widget_gallery
```

Schemer will identify the database and source versions it detected, then apply each update necessary:

```
Database version: 0
Source version: 3

Applying update #1 (/code/widget-project/database/1_test1-alpha.sql)...
Applying update #2 (/code/widget-project/database/2_test2-beta.sql)...
Applying update #3 (/code/widget-project/database/3_test3-gamma.sql)...

Up-to-date.
```

If an error is encountered during any update, `schemer` exits immediately. Remediate the problem, then issue the update command again to complete the operation.

## Help

Typing `schemer` or `schemer <command>` outputs usage and supported commands:

```
usage: schemer [--version] <command> <database-configuration-path> [<arguments>]

Supported commands are:
backup    Create backup
create    Create schema
restore   Restore backup
update    Update schema
```

