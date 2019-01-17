# Using psql

## connect
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

## psql

- to show users
    ~~~
    \du
    ~~~

- to show all databases
    ~~~
    \list
    ~~~

- to show all table in the current database
    ~~~
    \dl
    ~~~
