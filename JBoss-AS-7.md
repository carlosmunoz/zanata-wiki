## EAP 6.1 Alpha

EAP 6 Alphas are freely distributed. Download a copy at [jboss.org](http://www.jboss.org/jbossas/downloads). Once finished, extract the archive's contents. This location will be referenced as JBOSS7_HOME throughout the rest of the document.

EAP 6.1 Alpha contains the wrong hibernate libraries for Zanata development. Download hibernate 4.2.0.Final from [hibernate.org](www.hibernate.org) and replace Jboss hibernate libraries with these. In EAP 6.1, the hibernate module libraries are located at /usr/share/jbossas/modules/system/layers/base/org/hibernate/main

Replace the hibernate files labeled as 4.2.0.RC1 with their equivalent ones for 4.2.0.Final.

## EAP 6.1

For developers on RHEL, you can install EAP 6.1 by typing the following command on the terminal:

```sh
$ yum -y groupinstall "jboss-eap6"
```

This way of installing will place Jboss files under /usr/share/jbossas (and others at /etc/jbossas)

You can start EAP by typing the following command on a terminal:

```sh
$ service jbossas start
```

# Preparation:
## Create module for external Zanata settings
Create the file `$JBOSS7_HOME/modules/org/zanata/settings/main/module.xml`:
```sh
$ cd $JBOSS7_HOME
$ mkdir -p modules/org/zanata/settings/main
$ $EDITOR modules/org/zanata/settings/main/module.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<module xmlns="urn:jboss:module:1.1" name="org.zanata.settings">
    <resources>
        <resource-root path="."/>
    </resources>
</module>
```
Create the file `$JBOSS7_HOME/modules/org/zanata/settings/main/zanata.properties` (modify &lt;myusername&gt; to suit):
```properties
zanata.security.auth.policy.internal = zanata.internal
zanata.security.admin.users = <myusername>
```
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
## Configure security domain `zanata` in standalone.xml
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
                <security-domain name="zanata">
                    <authentication>
                        <login-module code="org.zanata.security.ZanataCentralLoginModule" flag="required"/>
                    </authentication>
                </security-domain>
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

The code snippet above is specific to internal authentication.


# Building and deploying:
```sh
$ git checkout seam230final
$ mvn install -DskipTests -Pnogwt
$ cp zanata-war/target/zanata-2.2-SNAPSHOT.war $JBOSS7_HOME/standalone/deployments/zanata.war
```
# Other things that might help
In `bin/standalone.conf`:
 * To increase memory for classes (and multiple redeployments), change -XX:MaxPermSize=256m to -XX:MaxPermSize=512m
 * To enable debugging, uncomment `JAVA_OPTS="$JAVA_OPTS -Xrunjdwp:transport=dt_socket,address=8787,server=y,suspend=n"`
 * To fix the [EAP 6 problem where most of the logging is missing](http://stackoverflow.com/questions/12670415/log4j-doesnt-log-anything-under-jboss-6-eap), add this line:

    JAVA_OPTS="$JAVA_OPTS -Dorg.jboss.as.logging.per-deployment=false"