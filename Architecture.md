Zanata is made up of many components that use various frameworks and are spread across several repositories. This section aims to give an overview of the major components that make up Zanata. To find the source code related to a component, see [[Repositories]].

# Overview
The server makes up the majority of Zanata's code and functionality, including the web editor and all database access code. Clients use the REST API to upload and download documents to and from the server, as well as some other less frequent operations.
![Architecture overview](http://zanata.org/images/diagrams/zanata-2.0-architecture-overview.svg)

# Server
The server can be broadly divided into the Project Pages, the Translation Editor, the REST API, and persistence. It is based on the Seam Framework, developed against JBoss and uses Hibernate, Hibernate Search, JSF, RichFaces, RESTEasy, Drools, Okapi Framework, GWT and more.

## Project Pages
The "Project Pages" refers to all the web pages presented by Zanata except the Translation Editor (which is separated here because its structure is very different from the others). The Project Pages allow users to authenticate, join language teams, find, create and interact with projects, upload and download documents and configuration files, and to launch the Translation Editor. The project pages use JSF and RichFaces for display.

See: [[Project Pages Architecture]]

## Translation Editor (Webtrans)
The Translation Editor is where the core translation work happens in Zanata. It allows translators to navigate through the documents of a project and enter or change translations, and provides a translation memory, search functionality, history viewing, chat, filters and more. It is built with GWT, so it is a JavaScript application compiled from Java source code and currently uses GWT-RPC to communicate with the server.

See: [[Webtrans Architecture]]

## REST API
The REST API is used by the clients, but also by some page components internally. We aim eventually to have all page and editor operations go through the REST API to ensure that it is fully expressive of Zanata's functionality, and the migration to this architecture is ongoing. The API uses RESTEasy and other libraries, and shares some components with the Maven Plugin and Command-Line Client.

See: [[REST API Architecture]]

## Persistence
Zanata uses Hibernate for persistence. Hibernate provides Object-Relational Mapping (ORM), mapping database tables to Java objects and greatly reducing the amount of SQL required for persistence. These objects ("Entities") are used for most server code, but data is converted into Data Transfer Objects (DTOs) before being sent to webtrans or over the REST interface.

See: [[Persistence Architecture]]



# Clients

The Command-Line Client and Maven plugin invoke the same set of commands, acting as wrappers to provide different methods of invocation and argument handling.

See: [[Client Command Architecture]]

## Maven Plugin
The Zanata Maven Plugin uses the Maven Plugin API to implement a plugin that can be incorporated into Maven workflows or used as a standalone command-line application (but still requiring a working Maven installation). The plugin uses various libraries including RESTEasy, JAX-RS, JAXB and so on.

See [[Maven Plugin Architecture]]

## Command-Line Client (in development)
The Command-Line Client shares much of its code with the Maven Plugin, but is intended to work as a standalone application without the need for installation of Maven.

See [[Command Line Client Architecture]]

## Python Client (end-of-life)
The Zanata Python Client is the only major component of Zanata that is not Java-based. As a result, it uses a completely separate code-base. It accesses the same REST API as the other clients, but does not use RESTEasy or Zanata's API library. Because a separate code-base adds unnecessary maintenance effort and more opportunity for bugs to arise, the Python Client is being phased out, to be replaced by a Java-based Command Line Client.