The Zanata Maven Plugin uses the Maven Plugin API to implement a plugin that can be incorporated into Maven workflows or used as a standalone command-line application (but still requiring a working Maven installation). The plugin is essentially a wrapper that handles invocation and argument handling, then passes control to classes in the `zanata-client-commands` project.

An introduction to Maven Plugins can be found at [Apache Maven Project - Guide to Developing Java Plugins](http://maven.apache.org/guides/plugin/guide-java-plugin-development.html).

It is recommended to understand the concept of a Command and an Options object in Zanata before continuing. See [[Client Command Architecture]].

Maven Plugin commands are implemented as Mojos. There is a Mojo for each available command in the Zanata Maven Plugin, e.g. `PutProjectMojo`. The Mojo acts as an Options object that is passed to an appropriate type of Command object.

## Control Flow
The Maven Plugin framework handles processing of parameters from .pom or the command line, and calls `execute()`. In `execute()`, a Command object is instantiated, given the Mojo as its Options implementation, and the command is run.

## Parameters
Parameters are defined with annotations as described in [Maven: The Complete Reference 11.5. Mojo Parameters](http://www.sonatype.com/books/mvnref-book/reference/writing-plugins-sect-mojo-params.html).
