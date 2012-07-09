# How to configure MySQL database for Zanata

# Install `MySQL`

- Run

    yum install mysql

 to install !MySql client on your machine.
- Run

    yum install mysql-server

 to install !MySql server on your machine. You may also want to run

    mysql_secure_installation

 to help secure your !MySql install


# JDBC Driver

- You will need `MySql` JDBC driver, Connector/J for JBoss AS.
- Download it from http://www.mysql.com/products/connector/j/.
- Then, untar/unzip it and extract the **jar** file.
- Copy the **jar** file into $JBOSS_HOME/server/default/lib.
  Or 
- Download through maven. Jar file will be in ~/.m2/repository/mysql/mysql-connector-java/5.1.9/mysql-connector-java-5.1.9.jar

    mvn -Pjboss5,mysql,nogwt hibernate3:hbm2ddl 

# Setup for Zanata

- Create a database in MySQL, named **zanata**, make sure you enable utf8 support when creating the database

      CREATE DATABASE zanata /*!40100 DEFAULT CHARACTER SET utf8 */;

- Start the MySQL database by running `/etc/init.d/mysqld start` from the command line

- To package Zanata build with MySQL database, use maven profile `mysql` during build.

    mvn -Pmysql clean install

# Setup for Exploded WAR deployment

For deployment of an exploded war, !MySql username and password should be updated in $USER_HOME/.m2/settings.xml as described in [JBoss Setup](http://code.google.com/p/flies/wiki/JBossSetup)