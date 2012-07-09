# Deploy flies on Fedora or Red Hat Enterprise Linux.

This wiki shows how to deploy flies on a Fedora/Red Hat Enterprise Linux.
We assume you have installed maven2 and java SDK.

# Prerequisites

## Useful yum repos

- [Jpackage](http://www.jpackage.org/): Generic Java in RPM form. Generally for JBoss.
   Follow the [instruction](http://www.jpackage.org/yum.php) for setting up yum repo.


# Deployment

This section shows the steps for flies deployment.
We use `$JBOSS_HOME` for JBoss AS installation directory,
and `$FLIES_SRC` for the flies source codes checkout from the mercurial repository. 

1. Install jbossas, which is mainly installed in /var/lib/jbossas

       yum install jbossas

1. cd $FLIES_SRC
1. Make sure following maven build succeed.

    mvn clean install -Pnogwt,jboss4 -DskipTests=true

1. sudo cp flies-war/target/classes/flies-ds.xml $JBOSS_HOME/server/default/deploy; sudo chown jboss:jboss $JBOSS_HOME/server/default/deploy/flies-ds.xml
1. sudo unzip flies-war/target/flies.war -d $JBOSS_HOME/server/default/deploy/flies.war; sudo chown -R jboss:jboss $JBOSS_HOME/server/default/deploy/flies.war
1. sudo cp ~/.m2/repository/com/h2database/h2/1.2.134/h2-1.2.134.jar $JBOSS_HOME/server/default/lib/; sudo chown -R jboss:jboss $JBOSS_HOME/server/default/lib/
 
# Run

You may either run the jboss as service:

    service jbossas start  

You may encounter error message, but flies may still be up and running at.
http://localhost:8080/flies

Or, run as normal command:

    $JBOSS_HOME/bin/run.sh

The flies instance should be run at 
http://localhost:8080/flies

If not, log files can be found in `/var/log/jbossas/default/`.