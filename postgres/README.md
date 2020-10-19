This file contains some useful commands required while using PostGres terminal.

# Start the PostGres terminal
```
psql postgres
```
NOTE : Depending on your computer settings `sudo` might be required.

In case you wanna check where is PSQL installed, use this command
```
username$ which psql
/usr/local/bin/psql
```

# Databases
## List all Databases

## Connect to a particular database
```
\c <db-name>
```
## Connect to a remote host
```
psql -h <host> -p <port> -d <db_name> -U <user>
```

## Tables
### List down all tables in a database
```
\dt
```
