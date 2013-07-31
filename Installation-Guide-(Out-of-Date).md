# Installation guide for Flies.

# Requirements

- JBoss EWP 5.1 (currently 5.1.2)
- MySQL5/InnoDB
- `zanata.war` (see the [Downloads](https://github.com/zanata/zanata/downloads) page.  The internalauth version lets users register as users in the Zanata database. The JAAS version can integrate with an existing authentication system, but you will need to configure JAAS yourself.)
- a datasource xml files with the jndi-name "zanataDatasource"

# Installation

- Install JBoss EWP 5.1, eg to `/opt/jboss-ewp-5.1`
- Install MySQL5
- create an empty database called `zanata`
- `CREATE DATABASE `zanata` /**!40100 DEFAULT CHARACTER SET utf8 **/;`
- copy `zanata.war` to `/opt/jboss-ewp-5.1/jboss-as-web/server/default/deploy`

Example datasource - filename `zanata-ds.xml`, goes in `/opt/jboss-ewp-5.1/jboss-as-web/server/default/deploy`

    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE datasources
        PUBLIC "-//JBoss//DTD JBOSS JCA Config 1.5//EN"
        "http://www.jboss.org/j2ee/dtd/jboss-ds_1_5.dtd">
    <datasources>
        <local-tx-datasource>
            <jndi-name>zanataDatasource</jndi-name>
            <connection-url>jdbc:mysql://localhost:3306/zanata?characterEncoding=UTF-8</connection-url>
            <driver-class>com.mysql.jdbc.Driver</driver-class>
            <check-valid-connection-sql>select 1 from dual</check-valid-connection-sql>
            <user-name>root</user-name>
            <password></password>
        </local-tx-datasource>
    </datasources>

Make sure the datasource points to the database you created above, with the correct username and password.

# Creating the Zanata database tables

1. If the username in your datasource has permission to modify tables and triggers, Zanata will create/update its tables on startup using the Liquibase servlet.  This is very handy for development, but it may not be a good idea in production.
1. Otherwise, you will need to install and run Liquibase 1.9.5 from the command-line before starting Zanata, something like this:

a. unzip zanata.war to zanata-war

b.

    classpath=$(echo zanata-war/WEB-INF/lib/*.jar | sed 's/ /:/g')':\
    zanata-war/WEB-INF/classes/:\
    /opt/jboss-ewp-5.1/jboss-as-web/server/default/lib/mysql-connector-java.jar:\
    /opt/jboss-ewp-5.1/jboss-as-web/lib/log4j-boot.jar'
    java -jar zanata-war/WEB-INF/lib/liquibase-core-1.9.5.jar --classpath=$classpath \
    --changeLogFile=db/db.changelog.xml \
    --username=dbuser --password=dbpass \
    --url='jdbc:mysql://localhost:3306/zanata?characterEncoding=UTF-8' \
    update

NB: You will need to adjust the filenames, and provide the username/password of a database user with the ability to create tables and triggers.

Also see [[Document Storage Directory]] for how to specify the required parameter for the document storage directory on the liquibase command line.

# Configuration
- configure document storage location before server startup: see [[Document Storage Directory]]
- visit `http://hostname.example.com/zanata/`
- Internal Authentication: just log in as `admin`/`admin`
- choose a secure password for admin (menu: My Profile / Change Password)
- JAAS/Kerberos: log in once to create an account, then use the `mysql` command line  to grant yourself the role `admin`.  See [[JAAS Authentication]].
- modify server configuration (menu: Administration / Server Configuration)
- set server URL, eg to `http://hostname.example.com/zanata`
- set the Admin Email address for email validation messages, eg `noreply@example.com` - don't forget to test this!
- set the URL for the Help link - eg a wiki page with instructions for new users - including how they can ask someone for access to Flies.
- JAAS/Kerberos: set the URL for the Register link - if there is no way of registering, you could link to a page which explains why.
- JAAS/Kerberos: set the default email domain for new users (default is `example.com`) - so if the user `bob` logs in, the suggested address will be `bob@example.com`
- Add at least one language (typically source language is `en-US`) at Administration -> Manage languages, so projects can be imported.

You will need to set up some way for users to be added to the 'translator' role, eg by introducing themselves on a mailing list, and appointing an admin to grant the 'translator' role.  

Alternatively, if you are happy for all users to enter translations, you can edit the `user` role, and add the `translator` role (by ticking the box).  Then anyone with an account will automatically have translator permissions.