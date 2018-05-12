# sandbag
**Manage MySQL or Percona Server containers in Docker, for testing**

This script is in early stages of development, and it's likely to change.

Sandbag makes it easy to create and manage sets of MySQL containers using Docker. 
You can run different versions, set up replication, and connect the MySQL client to containers.

This tool is inspired by the work in [dbdeployer](https://www.dbdeployer.com/)
and its predecessor [MySQL Sandbox](https://mysqlsandbox.net/),
but sandbag runs the MySQL instances in Docker containers.

This tool is intended to run MySQL or Percona Server containers only for testing, not for production use.
If you want to Docker for MySQL in production, best of luck, but don't ask me for help.

Tested with MySQL 5.6, 5.7, 8.0 Docker images, and Percona Server 5.6 and 5.7 Docker images.

If you want to use this script with MariaDB, best of luck, but don't ask me about it.

Dependencies
=

- Docker 18.05
- MySQL client 5.6 or similar
- bash
- openssl

Optional:

- Kitematic
- pv

Usage
=

    sandbag [ options ] COMMAND arguments...

Options
=

    sandbag -d DIRECTORY
    
Save container data and config files under _DIRECTORY_.
The default is `$HOME/sandbag`.

You can also control this with the environment variable `SANDBAG_HOME`.

    sandbag -i IMAGE
    
Use this option when creating a container with the `create` command (see below).
This is the name of an image, by default "percona/percona-server:5.6"

You can also control this with the environment variable `SANDBAG_IMAGE`.

Commands
=

    sandbag gencerts
    
Generate self-signed SSL certificates under `$SANDBAG_HOME/certs`.

Sandbag containers require SSL.
If you try to create a container with `create` before you generate certificates, you will get an error.

If you want to create your own certificates according to instructions in the MySQL manual,
you can do that, and place the certificates in `$SANDBAG_HOME/certs`.

    sandbag create NAME
    
Create a new container named _NAME_ to run MySQL, based on the current image.

Configuration files for the instance will be created under `$SANDBAG_HOME/conf`,
one for the server running inside a container,
and one for a client in the host environment to connect to the container.

The datadir for the instance is created under `$SANDBAG_HOME/data/NAME`.

    sandbag destroy NAME

Stop and decommission the named container.

    sandbag start NAME
    
Start the container _NAME_.

    sandbag stop NAME
    
Stop the container _NAME_.

    sandbag restart NAME
    
Stop and start the container _NAME_.

    sandbag status
    
List docker containers.

    sandbag use NAME
    
Open a MySQL client from your host environment, and connect to MySQL in the named container.
    
    sandbag dump NAME > DUMPFILE.gz

Export data using mysqldump, to a file _DUMPFILE.gz_.
The file will be output gzip-compressed.

    sandbag load NAME DUMPFILE.gz

Import data from a previously saved file _DUMPFILE.gz_.
The file must be gzip-compressed.

    sandbag replicate MASTER REPLICA
    
Create a container named _REPLICA_ if it does not exist.
Copy all data from the container named _MASTER_ and use it to initialize _REPLICA_.
This overwrites any data in _REPLICA_.

Configure MySQL replication between the two containers.
The configuration is for row-based replication, and GTID.

TODO
=

Add support for complementary tools such as the following, in no particular order.

* Gh-ost
* GhostFerry
* MySQL Shell
* MySQL Workbench
* MySQL Utilities
* Orchestrator
* Percona Monitoring and Management
* Percona XtraBackup
* Percona XtraDB Cluster
* ProxySQL
* Shift
* Sysbench
* Vitess

License
=

Apache License 2.0
