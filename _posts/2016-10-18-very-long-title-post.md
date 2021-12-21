---
title: Working with HSQLDB and Hybris
updated: 2015-10-22 23:37
---

## Synopsis

Hybris by default includes HSQLDB as the embedded DB for development mode, Out of the box option to query database includes running Flex or SQL query from the admin console.
In this blog we explore few other convenience options to query the database by making use of external SQL clients, we further explore the option of configuring Hybris with a standalone version of HSQL server

Hybris Version: 5.6.0.2
HSQL DB Version: 2.2.9
<div class="divider"></div>


## Embedded Database

By default hybris creates embedded database data file at ~/hybris5602/hybris/data/hsqldb/ , the data file name is mydb
We will provide this information while connecting from SQuirrel SQL Client or any other client of your choice.

jdbc:hsqldb:file:~/hybris5602/hybris/data/hsqldb/mydb;readonly=true


## Standalone database server

Download supported version of HSQL from http://sourceforge.net/projects/hsqldb/files/latest/download?source=typ_redirect 
You may refer following WIKI link to verify the supported version of DB https://wiki.hybris.com/display/release5/Third-Party+Compatibility+-+Release+5.0

Create server.properties with following content, this configuration will create a database with the name hybris running on port 9999



## Code Blocks

```javascript
#Hybris DB configuration file
# Databases:
server.database.0=file:hybris/hybris
server.dbname.0=hybrisdb
# =====
# Other configuration:
# Port
server.port=9999
# Show stuff on the console (change this to true for production):
server.silent=false
# Don't show JDBC trace messages on the console:
server.trace=false
server.no_system_exit=false
# Do not allow remote connections to create a database:
server.remote_open=true
# =====
```

<div class="divider"></div>

HSQLDB download does not include a startup script by default for MAC OS or Unix, you may create a startup script as follows.
#hsqldb/bin/runServer.sh
java -cp ../lib/hsqldb.jar org.hsqldb.server.Server --props ../../datafile/server.properties

<div class="divider"></div>

Make the script executable chmod 777 runServer.sh
Run the server by executing
./runServer.sh
This should start the new DB server on port 9999

Try connecting from SQuirrel SQL client first.
You may use username SA and blank password to connect, however, it is recommended to make use of a non  - DBA user account.
jdbc:hsqldb:hsql://localhost:9999/hybrisdb

Add following in the local.properties


db.url=jdbc:hsqldb:hsql://localhost:9999/hybrisdb
db.driver=org.hsqldb.jdbcDriver
db.username=HYBRIS
db.password=hybris
db.tableprefix=
hsqldb.usecachedtables=false


Perform ant all
Start server and initialize to load schema and tables to the new standalone database.
you should now be able to connect to the database using SQL Client as follows.

