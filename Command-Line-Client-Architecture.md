The Command-Line Client shares much of its code with the Maven Plugin, but uses a different mechanism to invoke command classes.

It is recommended to understand the concept of a Command and an Options object in Zanata before continuing. See [[Client Command Architecture]].

## Code Locations
The Zanata Command-Line Client has configuration and installation script in repository `zanataj`, which are responsible for the download and invocation of classes in project `zanata-cli` (found in repository `zanata-client`). The main entry-point for the Command-Line Client is `org.zanata.client.ZanataClient`, which is responsible for mapping command line parameters to appropriate Command classes found in project `zanata-client-commands` in package `org.zanata.client.commands`, which generally also use code in project `zanata-rest-client` and `zanata-common-api` (in repository `zanata-api`).

## Download & Installation Code (Ivy)
The repository `zanataj` does not contain any of the actual client code. `zanataj` contains a script that uses [Apache Ivy](http://ant.apache.org/ivy/) to retrieve the actual client code from Maven Central before invoking the client code.

## Control Flow
The Ivy script invokes `org.zanata.client.ZanataClient.main()`.`ZanataClient.main()` is responsible for ensuring that a valid command name has been entered, and causing valid commands to be run.

`ZanataClient` has Options objects mapped against command name, which are fed to `org.zanata.client.commands.ArgsUtil.process(...)` where the remaining command line parameters are processed and the Command object is instantiated and run. `ArgsUtil` is in project `zanata-client-commands` in repository `zanata-client`.

![Command-Line Client Flow of Control](http://zanata.org/images/diagrams/zanata-2.0-architecture-client-zanataj.svg)