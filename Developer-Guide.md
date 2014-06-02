# Zanata Developer Guide

 * [[JBoss AS 7|JBoss AS 7]]

# Build Status (Travis, master branch)

- zanata-parent [![Build Status](https://travis-ci.org/zanata/zanata-parent.svg?branch=master)](https://travis-ci.org/zanata/zanata-parent)
- zanata-api [![Build Status](https://travis-ci.org/zanata/zanata-api.svg?branch=master)](https://travis-ci.org/zanata/zanata-api)
- zanata-common [![Build Status](https://travis-ci.org/zanata/zanata-common.svg?branch=master)](https://travis-ci.org/zanata/zanata-common)
- zanata-client [![Build Status](https://travis-ci.org/zanata/zanata-client.svg?branch=master)](https://travis-ci.org/zanata/zanata-client)

# Development tools

- Java Development Kit (JDK) 1.6 - yum install java-1.6.0-openjdk-devel
- GIT - Source code repository
- Eclipse IDE for J2EE (Kepler 4.2)
- MySQL database
- JBoss AS 7.2 (EAP 6.1)
- Maven v3

# Code style (for new files)

See [[Coding Guide]].

# Zanata Developer Setup Guide


1. install JDK 1.6 (see above)
1. install git
1. get Zanata source [[GitHub Setup]]
1. [install Maven](Working-With-Maven#Installing_Maven_on_Fedora)
1. [install MySQL client and server](Database-Setup#Install_MySQL)
1. [create database: zanata](Database-Setup#Setup_for_Zanata)
1. [install JBoss](JBoss-Setup)
1. [configure JBoss](JBoss-Setup#JBoss_Configuration)
1. [add MySQL JDBC driver to JBoss](Database-Setup#JDBC_Driver)
1. [add Zanata datasource to JBoss](JBoss-Setup#Datasource)
1. [add mysql and JBoss details to maven settings](JBoss-Setup#Configuring_Zanata_to_deploy_to_JBoss_AS)
1. [install Eclipse](Eclipse-Setup#Getting_Eclipse)
1. [add Eclipse plugins](Eclipse-Setup#Recommended_Plugins)
1. [prepare zanata for eclipse import](Eclipse-Setup#Import_Zanata_Project)
1. import zanata into eclipse
1. [configure Eclipse](Eclipse-Setup#Configuration)


# Zanata Architecture Overview
See [[Architecture]]
![Architecture overview](http://zanata.org/images/diagrams/zanata-2.0-architecture-overview.svg)