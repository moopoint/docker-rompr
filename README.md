docker-rompr
=================

Out-of-the-box Rompr image (rompr + MySQL)

Credit
------

This work is based on Rompr by fatg3rman and a fork of tutumcloud/lamp

Usage
-----

To create the image `rawdlite/docker-rompr`, execute the following command on the docker-rompr folder:

	docker build -t rawdlite/docker-rompr .



Running your ROMPR docker image
------------------------------

Start your image binding the external ports 80 in all interfaces to your container:

	docker run -d -p 80:80 rawdlite/docker-rompr

Open in your Browser:

	http://localhost/

Hello Rompr!

Bind to another port
--------------------

In case you already have a webserver running under port 80 on your host you can bind an alternative port like 8080

	docker run -d -p 8080:80 rawdlite/docker-rompr

Open in your Browser:

        http://localhost:8080

Debug
=====

Check php variables:

       http://localhost/phpinfo.php

Entering the container
----------------------

Get the container name or id

	docker ps

run a shell in the container

	docker exec -it <name or id> /bin/bash


Connecting to the bundled MySQL server from within the container
----------------------------------------------------------------

The bundled MySQL server has a `root` user with no password for local connections.
You can enter the database as root:

	mysql

Connecting to the bundled MySQL server from outside the container
-----------------------------------------------------------------
To expose the mysql port run the container as.

	docker run -d -p 80:80 -p 3306:3306 rawdlite/docker-rompr

The first time that you run your container, a new user `admin` with all privileges
will be created in MySQL with a random password. To get the password, check the logs
of the container by running:

	docker logs $CONTAINER_ID

You will see an output like the following:

	========================================================================
	You can now connect to this MySQL Server using:

	    mysql -uadmin -p47nnf4FweaKu -h<host> -P<port>

	Please remember to change the above password as soon as possible!
	MySQL user 'root' has no password but only allows local connections
	========================================================================

In this case, `47nnf4FweaKu` is the password allocated to the `admin` user.

You can then connect to MySQL:

	 mysql -uadmin -p47nnf4FweaKu -h 127.0.0.1

Remember that the `root` user does not allow connections from outside the container -
you should use this `admin` user instead!


Setting a specific password for the MySQL server admin account
--------------------------------------------------------------

If you want to use a preset password instead of a random generated one, you can
set the environment variable `MYSQL_PASS` to your specific password when running the container:

	docker run -d -p 80:80 -p 3306:3306 -e MYSQL_PASS="mypass" rawdlite/docker-rompr

You can now test your new admin password:

	mysql -uadmin -p"mypass" -h 127.0.0.1


