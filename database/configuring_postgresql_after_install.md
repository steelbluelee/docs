# How to configure postgresql after installation

1. change the password of the postgres user<BR>
    in command line prompt
    ~~~
    ~# sudo -u postgres psql
    ~# ALTER USER postgres with encrypted password 'my_password';
    ~~~
    The \password is always stored encrypted in the system catalogs.
    The ENCRYPTED keyword has no effect, but is accepted for backwards compatibility.

1. edit /var/lib/pgsql/11/data/pg_hba.conf file
    ~~~
    # TYPE  DATABASE        USER            ADDRESS                 METHOD

    # "local" is for Unix domain socket connections only
    local   all             all                                     peer
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

    host    all             all             0.0.0.0/0               md5
    # IPv6 local connections:
    host    all             all             ::1/128                 md5
    # Allow replication connections from localhost, by a user with the
    # replication privilege.
    local   replication     all                                     peer
    host    replication     all             127.0.0.1/32            ident
    host    replication     all             ::1/128                 ident
    ~~~


1. edit /var/lib/pgsql/11/data/postgresql.conf file

    ~~~
    listen_addresses = '*'          # what IP address(es) to listen on; 
    ~~~

1. start postgresql server
    ~~~
    ~# systemctl restart postgresql-11
    ~~~


1. add the following firewall rule
    ~~~
    ~# sudo firewall-cmd --permanent --zone=public --add-port=5432/tcp 
    ~# firewall-cmd --reload
    ~~~
    or
    ~~~
    ~# firewall-cmd --permanent --zone=trusted --add-source=<Client IP address>/32
    ~# firewall-cmd --permanent --zone=trusted --add-port=5432/tcp
    ~# firewall-cmd --reload
    ~~~

1. add a database user and create a database
    ~~~
    ~# sudo -u postgres psql postgres
    postgres=# create user sentry;
    postgres=# create database sentrydb ENCODING 'UTF-8' ;
    postgres=# GRANT ALL PRIVILEGES ON DATABASE sentrydb TO sentry;
    postgres=# \password sentry
    ~~~
    cf)
    ~~~
    postgres=# create user sentry WITH SUPERUSER PASSWORD 'sentry';
    ~~~

1. you can access the database that you've just created with the new user using psql
    - in linux
       ~~~
       psql \
            --host=<DB instance endpoint> \
            --port=<port> \
            --username=<master user name> \
            --password \
            --dbname=<database name> 
       ~~~
    - in windows
        ~~~ 
        psql ^
            --host=<DB instance endpoint> ^
            --port=<port> ^
            --username=<master user name> ^
            --password ^
            --dbname=<database name> 
         ~~~

1. install pgadmin4 in my labtop
    ~~~
    ~$ sudo dnf install pgadmin4
    ~~~

1. start pgadmin4
    ~~~
    ~$ sudo python3 /usr/lib/python3.7/site-packages/pgadmin4-web/pgAdmin4.py
    ~~~

1. access http://localhost:5050 with a web browser

## references
- https://blog.programster.org/configuring-postgresql-after-installation
- https://devops.profitbricks.com/tutorials/install-postgresql-on-centos-7/
- https://www.lesstif.com/pages/viewpage.action?pageId=31850584
