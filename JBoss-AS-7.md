(Not working yet, under development)

# Preparation:
## Create module for external Zanata settings
Create the file `$JBOSS7_HOME/modules/org/zanata/settings/main/module.xml`:
```sh
$ cd $JBOSS7_HOME
$ mkdir -p modules/org/zanata/settings/main
$ $EDITOR modules/org/zanata/settings/main/module.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<module xmlns="urn:jboss:module:1.1" name="org.zanata.settings">
    <resources>
        <resource-root path="."/>
    </resources>
</module>
```
Create the file `$JBOSS7_HOME/modules/org/zanata/settings/main/zanata.properties` (modify &lt;myusername&gt; to suit):
```
zanata.security.auth.policy.internal = zanata.internal
zanata.security.admin.users = <myusername>
```
## Make JavaMelody work on AS7
Modify the file `$JBOSS7_HOME/modules/sun/jdk/main/module.xml` to insert 
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
$ mkdir -p modules/mysql/main
$ ln -s /usr/share/java/mysql-connector-java.jar modules/mysql/main
$ $EDITOR modules/mysql/main/module.xml
```
```xml
<module xmlns="urn:jboss:module:1.1" name="mysql">
    <resources>
        <resource-root path="mysql-connector-java.jar"/>
    </resources>
    <dependencies>
        <module name="javax.api"/>
        <module name="javax.transaction.api"/>
    </dependencies>
</module>
```

Reference: http://jaitechwriteups.blogspot.com.au/2012/02/jboss-as-710final-thunder-released-java.html

## Create a datasource for Zanata:
Create the file `$JBOSS7_HOME/standalone/deployments/zanata-ds.xml` (modify to suit):
```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!-- http://docs.jboss.org/ironjacamar/schema/datasources_1_0.xsd -->
    <!--
    Using this datasource:
    1. create a jboss module for mysql-connector and activate it using jboss-cli.sh
    2. save this datasource as JBOSS_HOME/standalone/deployments/zanata-ds.xml
    See http://jaitechwriteups.blogspot.com.au/2012/02/jboss-as-710final-thunder-released-java.html
    -->
    <datasources>
      <datasource jndi-name="java:zanataDatasource"
        enabled="true" use-java-context="true" pool-name="zanataDatasource">
        <connection-url>jdbc:mysql://localhost:3306/zanata?characterEncoding=UTF-8</connection-url>
        <driver>mysql-5-driver</driver>
        <security>
           <user-name>root</user-name>
           <password></password>
        </security>
      </datasource>
    </datasources>
```
## Configure security domain `zanata.internal` in standalone.xml
 * TODO
 * https://community.jboss.org/wiki/JBossAS7SecurityDomainModel
 * edit `standalone.xml`:
```sh
$ $EDITOR standalone/configuration/standalone.xml
```
```xml
...
            <security-domains>
...
                <security-domain name="zanata.internal">
                    <authentication>
                        <login-module code="org.jboss.seam.security.jaas.SeamLoginModule" flag="required"/>
                    </authentication>
                </security-domain>
...
            </security-domains>
...
```
If jboss is running, tell it to apply the change with:
```sh
$ ./jboss-cli.sh -c :reload
```


# Building and deploying:
```sh
$ git checkout seam230final
$ mvn install -DskipTests -Pnogwt
$ cp zanata-war/target/zanata-2.2-SNAPSHOT.war $JBOSS7_HOME/standalone/deployments/zanata.war
```
# Other things that might help
In `bin/standalone.conf`:
 * To increase memory for classes, change -XX:MaxPermSize=256m to -XX:MaxPermSize=512m
 * To enable debugging, uncomment `JAVA_OPTS="$JAVA_OPTS -Xrunjdwp:transport=dt_socket,address=8787,server=y,suspend=n"`


### Rationale
Apparently it is possible, but not recommended, to run Seam 2.2 apps on AS 7, by packaging Hibernate 3.x with the application.  However, we want to upgrade Hibernate and Hibernate Search, so we need to upgrade to Seam 2.3...

Seam 2.3 requires JSF 2 (early Seam 2.3 alphas may have worked with JSF 1, but this changed before Final).  For now, we are trying to use RichFaces 3.3.3's JSF 2 support.