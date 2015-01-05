## JBoss EAP 6.2

You can [download JBoss EAP 6.2](http://www.jboss.org/jbossas/downloads/) to use with Zanata. Extract the archive. This location will be known as JBOSS_HOME for here on. You can start JBoss by typing the following on your command line.

```sh
$ cd $JBOSS_HOME
$ ./bin/standalone.sh
```

For developers on Red Hat Enterprise Linux, you can install JBoss EAP 6.2 by typing the following command on the terminal:

```sh
$ yum -y groupinstall "jboss-eap6"
# or:
$ yum -y install jbossas-standalone

```

This way of installing will place Jboss files under 
* /usr/share/jbossas (JBOSS_HOME)
* /etc/jbossas
* /var/lib/jbossas (shortcuts in /usr/share/jbossas)

You can start JBoss EAP by typing the following command on a terminal:

```sh
$ service jbossas start
```

# Preparation:
## Add JNDI entries for `zanata` in standalone.xml
Search `subsystem xmlns="urn:jboss:domain:naming:1.3"` and add bindings as following. Adjust the paths/emails accordingly.

```xml
<subsystem xmlns="urn:jboss:domain:naming:1.3">
    <bindings>
        <simple name="java:global/zanata/security/auth-policy-names/internal" value="zanata.internal"/>
        <simple name="java:global/zanata/security/admin-users" value="admin"/>
        <simple name="java:global/zanata/files/document-storage-directory" value="/example/path"/>
        <simple name="java:global/zanata/email/default-from-address" value="no-reply@zanata.org"/>
    </bindings>
    <remote-naming/>
</subsystem>
```
Optional entries that can be defined in JNDI please refer to source org.zanata.config.JndiBackedConfig.

## Make JavaMelody work on AS7
Modify the file `$JBOSS7_HOME/modules/system/layers/base/sun/jdk/main/module.xml` to insert 
```xml
<path name="com/sun/management"/>
```
immediately after
```xml
<paths>
```
## Install MySQL driver
```sh
$ sudo yum install mysql-connector-java
$ cd $JBOSS7_HOME
$ ln -s /usr/share/java/mysql-connector-java.jar standalone/deployments/
```
Reference: http://jaitechwriteups.blogspot.com/2012/02/jboss-as-710final-thunder-released-java.html

## Create a datasource for Zanata:
Create the file `$JBOSS7_HOME/standalone/deployments/zanata-ds.xml` (modify to suit):
```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!-- http://docs.jboss.org/ironjacamar/schema/datasources_1_0.xsd -->
    <!--
    Using this datasource:
    1. create a jboss module for mysql-connector and activate it using jboss-cli.sh
    2. save this datasource as JBOSS_HOME/standalone/deployments/zanata-ds.xml
    See http://jaitechwriteups.blogspot.com/2012/02/jboss-as-710final-thunder-released-java.html
    -->
    <datasources>
      <datasource jndi-name="java:jboss/datasources/zanataDatasource"
        enabled="true" use-java-context="true" pool-name="zanataDatasource">
        <connection-url>jdbc:mysql://localhost:3306/zanata?characterEncoding=UTF-8</connection-url>
        <driver>mysql-connector-java.jar</driver>
        <security>
           <user-name>root</user-name>
<!--
           <password></password>
-->
        </security>
      </datasource>
    </datasources>
```

## Configure zanata properties in `standalone.xml`
```ssh
$ $EDITOR standalone/configuration/standalone.xml
```
```xml
...
            <extensions>
...
            </extensions>
            <system-properties>
	         <property name="javamelody.storage-directory" value="${user.home}/stats"/>
    	         <property name="hibernate.search.default.indexBase" value="${user.home}/indexes"/>
            </system-properties>
...
```
These properties should be set to the location where Zanata will store system statistics, and search indexes respectively.

## Configure security domain `zanata` in standalone.xml
See [[JAAS-Authentication]]

If jboss is running, tell it to apply the change with:
```sh
$ ./jboss-cli.sh -c :reload
```

The code snippet above is specific to internal authentication.

# Running Integration Tests

To run integration tests (`mvn integration-test` or `mvn verify`), a fresh instance of AS 7 is required. Modify the `arquillian.xml` file in `zanata-server`. The `jbossHome` property should point to the location of this new AS7 instance. The integration tests will deploy all needed files, start and stop the instance. Alternatively, this property may be removed and the JBOSS_HOME environment variable may be used.

# Building and deploying:
```sh
$ git checkout seam230final
$ mvn clean install -DskipTests -Pnogwt
$ cp zanata-war/target/zanata-2.2-SNAPSHOT.war $JBOSS7_HOME/standalone/deployments/zanata.war
```
# Other things that might help
In `bin/standalone.conf`:
 * To increase memory for classes (and multiple redeployments), change -XX:MaxPermSize=256m to -XX:MaxPermSize=512m
 * To enable debugging, uncomment `JAVA_OPTS="$JAVA_OPTS -Xrunjdwp:transport=dt_socket,address=8787,server=y,suspend=n"`
 * To fix the [JBoss EAP 6 problem where most of the logging is missing](http://stackoverflow.com/questions/12670415/log4j-doesnt-log-anything-under-jboss-6-eap), add this line:

    JAVA_OPTS="$JAVA_OPTS -Dorg.jboss.as.logging.per-deployment=false"