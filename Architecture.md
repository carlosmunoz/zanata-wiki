Zanata is made up of many components that use various frameworks and are spread across several repositories. This section aims to give an overview of the major components that make up Zanata. To find the source code related to a component, see [[SourceCodeStructure]].

# Overview
The server makes up the majority of Zanata's code and functionality, including the web editor and all database access code. Clients use the REST API to upload and download documents to and from the server, as well as some other less frequent operations.
![Architecture overview](http://zanata.org/images/diagrams/zanata-1.7-architecture-overview.svg)

# Server
The server can be broadly divided into persistence, the REST API, the Project Pages, and the Translation Editor. It is based on the Seam Framework and uses Hibernate, Hibernate Search, JSF, RichFaces, RESTEasy, Drools, Okapi Framework, GWT and more.

## Persistence
## REST API
## Project Pages
## Translation Editor (Webtrans)


# Clients

## Maven Plugin
The Zanata Maven Plugin uses the Maven Plugin API to implement a plugin that can be incorporated into Maven workflows or used as a standalone command-line application (but still requiring a working Maven installation). The plugin uses various libraries including RESTEasy, JAX-RS, JAXB and so on.

## Python Client (end-of-life)
The Zanata Python Client is the only major component of Zanata that is not Java-based. As a result, it uses a completely separate code-base. It accesses the same REST API as the other clients, but does not use RESTEasy or Zanata's API library. Because a separate code-base adds unnecessary maintenance effort and more opportunity for bugs to arise, the Python Client is being phased out, to be replaced by a Java-based Command Line Client.

## Command Line Client (in development)
The Command Line Client shares much of its code with the Maven Plugin, but is intended to work as a standalone application without the need for installation of Maven.