# How to setup and configure JBoss AS for Zanata development

# Getting and Installing JBoss

You can use either JBoss AS 4.2.3 from jboss.org (no longer tested), or JBoss EWP 5. JBoss AS is available for download from [jboss.org](http://www.jboss.org/jbossas/downloads.html)(choose jboss-4.2.3.GA.zip)

Download and unzip JBossAS to a location of your choice. We will refer to this location as `$JBOSS_HOME`.

# JBoss Configuration

## Optimized settings for re-deployment

Edit your copy of jboss/bin/run.conf (run.conf.bat for Windows) like so: 

    #old line
    #JAVA_OPTS="-Xms128m -Xmx512m -Dsun.rmi.dgc.client.gcInterval=3600000 -Dsun.rmi.dgc.server.gcInterval=3600000"
    JAVA_OPTS="-Xms128m -Xmx512m -XX:PermSize=256m -Dsun.rmi.dgc.client.gcInterval=3600000 
    -Dsun.rmi.dgc.server.gcInterval=3600000 -XX:+UseConcMarkSweepGC -XX:+CMSPermGenSweepingEnabled 
    -XX:+CMSClassUnloadingEnabled -XX:MaxPermSize=512m -Xverify:none"

# Enabling Remote Debugging

To enable debugging your JBoss application, uncomment the following `JAVA_OPTS` line in `$JBOSS_HOME/bin/run.conf`

    # Sample JPDA settings for remote socket debugging
    #JAVA_OPTS="$JAVA_OPTS -Xrunjdwp:transport=dt_socket,address=8787,server=y,suspend=n"

## Configuring Zanata to deploy to JBoss AS

Edit your `$USER_HOME/.m2/settings.xml` and ensure the `as.deploy` property points to your JBoss installation (for example, assuming jboss EWP 5.0 is installed in ~/apps/jboss-ewp-5.0). If `$USER_HOME/.m2/settings.xml` does not exist, you can copy the default template from `$M2_HOME/conf/settings.xml`. Edit the `mysql.user` and `mysql.password` properties to match your MySql settings.

    <profile>
      <id>default</id>
      <activation>  
        <activeByDefault>true</activeByDefault>
      </activation>
      <properties>
        <jboss.home>${user.home}/apps/jboss-ewp-5.0/jboss-as-web</jboss.home>
        <as.deploy>${jboss.home}/server/default/deploy</as.deploy>
        <mysql.user>root</mysql.user>
        <mysql.password></mysql.password>
      </properties>
    </profile>

Here is a complete sample of .m2/settings.xml:

    <settings xmlns="http://maven.apache.org/POM/4.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                          http://maven.apache.org/xsd/settings-1.0.0.xsd">
      <mirrors>
        <mirror>
          <id>ibiblio.org</id>
          <name>ibiblio Mirror of http://repo1.maven.org/maven2/</name>
          <url>http://mirrors.ibiblio.org/pub/mirrors/maven2</url>
          <mirrorOf>central</mirrorOf>
        </mirror>
      </mirrors>
      <profiles>
        <profile>
          <id>default</id>
            <activation>
              <activeByDefault>true</activeByDefault>
            </activation>
          <properties>
            <jboss.home>${user.home}/apps/jboss-4.2.3.GA</jboss.home>
            <env.JBOSS_HOME>${jboss.home}</env.JBOSS_HOME>
            <as.deploy>${jboss.home}/server/default/deploy</as.deploy>
            <env.hibernate.show_sql>false</env.hibernate.show_sql>
            <mysql.user>root</mysql.user>
            <mysql.password></mysql.password>
          </properties>
          <repositories>
    		<!-- NEW jboss.org snapshot repo  -->
    		<repository>
    			<id>repository.jboss.org-snapshots</id>
    			<name>JBoss Snapshots Repository (Nexus)</name>
    			<url>https://repository.jboss.org/nexus/content/repositories/snapshots</url>
    			<releases>
    				<enabled>false</enabled>
    			</releases>
    			<snapshots>
    				<enabled>false</enabled>
    				<!-- We don't want to download snapshots by default
            			http://community.jboss.org/thread/147383
    					Use mvn -U package to update snapshots. -->
    				<updatePolicy>never</updatePolicy>
    			</snapshots>
    		</repository>
          </repositories>
        </profile>
      </profiles>
    </settings>

## Datasource

MySQL username and password from settings.xml (see above) are used in `$JBOSS_HOME/server/default/deploy/zanata-ds.xml`, which is copied from `zanata/server/zanata-war/src/main/resources-jboss` when deploying an exploded war. The structure of `zanata-ds.xml` is shown here for reference.

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
            <user-name>${mysql.user}</user-name>
            <password>${mysql.password}</password>
        </local-tx-datasource>
    </datasources>


 

Refer to http://community.jboss.org/wiki/SetUpAMysqlDatasource if you want to set up a datasource yourself. 

# Running JBoss

You start JBoss by running `$JBOSS_HOME/bin/run.sh` (`$JBOSS_HOME/bin/run.bat` on Windows) from the command line . You stop JBoss by entering `CTRL+c`.

## Building and Deploying (Zanata 1.5+)

Run the following command for a full zanata build:

    mvn clean install

To deploy to an exploded war directory and deploy zanata-ds.xml to your jboss directory (see config and datasource sections above), run:
    mvn -Pexplode -DinternalAuth -DskipTests clean package

Hint: for quicker builds, enable the profile `nogwt` (_don't compile for GWT, handy when using DevMode_), `firefox` (_compile GWT for Firefox only_) or `chrome` (_compile GWT for Chrome/WebKit only_).  For instance:

    mvn -Pexplode,chrome -DinternalAuth -DskipTests clean package


### Multiple Authentication schemes (Zanata 1.5+)

The default profile will build multiple war files for each authentication mechanism. The maven property 'internalAuth' will ensure that only the internal authentication war is built.

<table>
  <tr><td>*Maven Profile*</td><td>*Description*</td><td>*Artifact Name*</td></tr>
  <tr><td>_(default)_</td><td>All Artifacts</td><td>(See Below)</td></tr>
  <tr><td>`fedora`</td><td>Fedora OpenID</td><td>zanata-{version}-fedora.war</td></tr>
  <tr><td>`kerberos`</td><td>Kerberos</td><td>zanata-{version}-kerberos.war</td></tr>
  <tr><td>`jaas`</td><td>Pure JAAS</td><td>zanata-{version}-jaas.war</td></tr>
  <tr><td>`autotest`</td><td>Internal Authentication + H2 (Used for testing only)</td><td>zanata-{version}-autotest.war</td></tr>
  <tr><td>`-DinternalAuth`</td><td>Internal Authentication</td><td>zanata-{version}-internal.war</td></tr>
</table>

See also [[JAASAuthentication]].


## Building and Deploying (Zanata 1.4)

Run the following command for a full build of `zanata.war`:
    mvn -Pmysql,jboss5,internalauthentication clean install

To deploy to an exploded war directory and deploy zanata-ds.xml to your jboss directory (see config and datasource sections above), run:
    mvn -Pmysql,jboss5,explode,internalauthentication -DskipTests clean package

Hint: for quicker builds, enable the profile `nogwt` (*don't compile for GWT, handy when using DevMode*), `firefox` (*compile GWT for Firefox only*) or `chrome` (*compile GWT for Chrome only*).  For instance:

    mvn -Pmysql,jboss5,explode,internalauthentication,chrome -DskipTests clean package

### Choosing Authentication scheme (Zanata 1.4)

NB: the profile `internalauthentication` is not needed when building if you have configured JAAS in your jboss install.  

<table>
  <tr><td>**Maven Profile**</td><td>**Description**</td></tr>
  <tr><td>*(default)*</td><td>Pure JAAS</td></tr>
  <tr><td>`internalauthentication`</td><td>internal authentication database</td></tr>
  <tr><td>`kerberos`</td><td>Kerberos</td></tr>
  <tr><td>`fedora`</td><td>Fedora OpenID</td></tr>
</table>

See also [JAASAuthentication].

## If deployment to local JBoss server failed, what should I do?

Firstly, you should stop the running JBoss by entering `CTRL+C`. Check if the JBoss is still in memory with `ps -A |grep run.sh`. If it is, you can terminate the process with `pkill run.sh` and kill the java with the pid right smaller than run.sh. (Ed: what??)

If deployment still failes after restarting JBoss, you can manually remove the previous deployment with `rm -rf $JBOSS_HOME/server/default/deploy/zanata*` (replace `$JBOSS_HOME` with location of your JBoss).