# Maven settings for Zanata

# Overview

This document outlines the usage of maven within the zanata-war project.


# Installing Maven on Fedora

Download Maven from [Version 3.0.x is recommended.

## ... as root

Extract the archive to e.g. `/opt/apache-maven-3.0`.

You then need to ensure the `mvn` command is in your path. As root, create a file such as `/etc/profile.d/maven.sh` with the following contents:

    export MAVEN_HOME=/opt/apache-maven-3.0
    export PATH=$PATH:$MAVEN_HOME/bin

After restarting your console-session, you should now be able to run `mvn`.

## ... as user

Extract the archive to e.g. `~/apps/apache-maven-3.0`.

You then need to ensure the `mvn` command is in your path. Add these lines to your .bashrc (or similar)

    export MAVEN_HOME=~/apps/apache-maven-3.0
    export PATH=$PATH:$MAVEN_HOME/bin

After restarting your console-session, you should now be able to run `mvn`.


# Maven Profiles used with Zanata (zanata-war)

- `jboss4` - Profile with settings for jboss4 - also needed for integration tests
- `explode` - Enables exploded deployment during the 'package' phase
- `dev` - Activates common development settings (enable debug, import test data)
- `mysql` - Activates settings for using mysql as the datasource (H2 is the default)
- `eclipse` - Activates common development settings for eclipse
- `nogwt` - Skips GWT compilation (for use with DevMode)
- `chrome` - speeds up GWT compilation by targeting Safari/Chrome browsers only
- `firefox` - speeds up GWT compilation by targeting Mozilla browsers only
- `internalauthentication` - Use internal authentication (JAAS authentication is default)
- `zanata-test` - Generate zanata-test.war instead of zanata.war

# Useful command-line options

- `-Dgwt.validateOnly` - check GWT modules but don't compile them
- `-Dgwt.compiler.skip` - another (more standard) way of skipping GWT compilation
- `-Dtest=MyTest` - runs only tests which match `*MyTest*`; useful for re-running a unit test
- `-Dit.test=MyTest` - runs only tests which match `*MyTest*`; useful for re-running an integration test
- `-Darquillian.jboss.home=/my/jboss` - When running integration tests, tells maven where to find the jboss server to use.

# Common Commands

### Generate Eclipse Configuration

    mvn -Peclipse,dev,nogwt -DskipTests=true install eclipse:clean eclipse:eclipse

### Exploded deployment to local JBoss server

    mvn -Pexplode -DskipTests package

or more commonly one of these (disable GWT compilation; enable debug and testdata):

    mvn -Pexplode,nogwt -DskipTests package
    mvn -Pchrome -DskipTests package

### Run integration tests

    mvn -Pnogwt -Darquillian.jboss.home=/my/jboss verify

## Using the JBoss.org Maven Repositories (optional reading)

See [http://community.jboss.org/wiki/MavenGettingStarted-Users](http://maven.apache.org/download.html].)