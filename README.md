docker-lamp53
=================

Out-of-the-box LAMP image (PHP+MySQL+PhantomJS)


Usage
-----

Clone the repository
	git clone https://github.com/bixsolutions/docker-lamp53

To create the image `bix_com_uy/lamp53`, execute the following command on the tutum-docker-lamp folder:

	docker build -t bix_com_uy/lamp53:latest .


Running your LAMP docker image
------------------------------

Start your image binding the external ports 80 and 3306 in all interfaces to your container:

	docker run -d -p 80:80 -p 3306:3306 -p 2222:22 \
    		-v /path/to/project:/var/www/html \
    		-v /path/to/project/logs:/var/log/apache2 \
    		-v /path/to/mysql-data:/var/lib/mysql \
    		bix_com_uy/lamp53:latest

Test your deployment:

	curl http://localhost/

Hello world!


Connecting to the bundled MySQL server from outside the container
-----------------------------------------------------------------

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

	 mysql -uadmin -p47nnf4FweaKu

Remember that the `root` user does not allow connections from outside the container - 
you should use this `admin` user instead!


Setting a specific password for the MySQL server admin account
--------------------------------------------------------------

If you want to use a preset password instead of a random generated one, you can
set the environment variable `MYSQL_PASS` to your specific password when running the container:

	docker run -d -p 80:80 -p 3306:3306 -e MYSQL_PASS="mypass" tutum/lamp

You can now test your new admin password:

	mysql -uadmin -p"mypass"
