# Using the Maven plugin for Zanata integration

# Introduction

The zanata plugin provides several goals under the zanata: prefix. For example:

    mvn zanata:help
    mvn zanata:listremote
    mvn zanata:putuser
    mvn zanata:putproject
    mvn zanata:putversion
    mvn zanata:push
    mvn zanata:pull

# Installing Apache Maven

For F16+ and RHEL 7+: `yum install maven`

For RHEL5/6, follow the following commands:
   
```bash
sudo wget -O /etc/yum.repo.d/epel-apache-maven.repo  http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo
yum install apache-maven
```


The above packages provide the executable `mvn` which can invoke zanata-maven-plugin.

# Configuring the Maven Plugin
To use shorthand command like `mvn zanata:help` instead of long command like `mvn org.zanata:zanata-maven-plugin:help` 

Download the [settings.xml](https://raw.github.com/wiki/zanata/zanata-server/m2/settings.xml) as `~/.m2/settings.xml by:
```bash
wget -O ~/.m2/settings.xml https://raw.github.com/wiki/zanata/zanata-server/m2/settings.xml
```
See [Configuring Maven Plugin](http://zanata.org/help/maven-plugin/maven-plugin-config/) for details.

# Activating the plugin
Note that zanata-maven-plugin 2.0.0 requires Zanata server 2.0+.  If you need to connect to Zanata 1.6/1.7, use 1.7.5 instead.

To activate the "zanata:" prefix, you should create/edit your Maven project's pom.xml like this:

For better performance with Zanata 2.x servers:
    <project>
    ...
      <build>
        <plugins>
          <plugin>
            <groupId>org.zanata</groupId>
            <artifactId>zanata-maven-plugin</artifactId>
            <version>2.0.1</version>
          </plugin>
        </plugins>
      </build>
    ...
    </project>

For older Zanata 1.x servers:

    <project>
    ...
      <build>
        <plugins>
          <plugin>
            <groupId>org.zanata</groupId>
            <artifactId>zanata-maven-plugin</artifactId>
            <version>1.7.5</version>
          </plugin>
        </plugins>
      </build>
    ...
    </project>

for instance, here's a complete sample pom.xml you can use:

# Sample pom.xml

    <project>
       <modelVersion>4.0.0</modelVersion>
       <groupId>null</groupId>
       <artifactId>null</artifactId>
       <version>0</version>
       
       <build>
          <plugins>
             <plugin>
                <groupId>org.zanata</groupId>
                <artifactId>zanata-maven-plugin</artifactId>
                <version>2.0.1</version>
                <configuration>
                   <srcDir>.</srcDir>
                </configuration>
             </plugin>
          </plugins>
       </build>
    </project>


or you can just use the long version:

    mvn org.zanata:zanata-maven-plugin:help


# Details

The plugin supports the standard Zanata ClientConfiguration files, with the ability to override configuration using system properties or plugin configuration parameters (in pom.xml).

Some of the mojos (currently just publican-push) may prompt the user for confirmation before making changes to Zanata.  Use Maven's -B/--batch-mode option to disable prompts when writing scripts. 

Note: Maven 2.2 does **not** support overriding pom configuration with system properties.  See http://jira.codehaus.org/browse/MNG-1992  So if you hard-code a value in the pom (like `<srcDir>.</srcDir>`) you won't be able to override it from the command line.  But you could try something like this:

       <properties>
          <zanata.srcDir>.</zanata.srcDir>
       </properties>
       
       <build>
          <plugins>
             <plugin>
                <groupId>org.zanata</groupId>
                <artifactId>zanata-maven-plugin</artifactId>
                <version>2.0.1</version>
                <configuration>
                   <srcDir>${zanata.srcDir}</srcDir>
                </configuration>
             </plugin>
          </plugins>
       </build>


The pom configuration parameters are in camelCase.  Note: When specifying these values as system properties from the command line, you will need to prepend "zanata." to the key.  

For example, in the pom:

       <configuration>
          <importPo>true</importPo>
          <srcDir>.</srcDir>
       </configuration>

and the command-line equivalent:

    mvn zanata:publican-push -Dzanata.importPo -Dzanata.srcDir=.


NB: the commands `listremote`, `putuser`, `putproject` and `putversion` are experimental, and subject to significant change.  (For instance, the names of their system properties are currently quite different from the camel case parameter names.)

General information about Maven plugin configuration can be found [here](http://maven.apache.org/guides/mini/guide-configuring-plugins.html).
 

The command

    mvn zanata:help

will give an overview of the goals which are available in the plugin.

    mvn zanata:help -Ddetail=true

lists all goals and their parameters.

# Reference Guide

The latest documentation of the plugin's goals and parameters is linked here: http://zanata.org/documentation.html