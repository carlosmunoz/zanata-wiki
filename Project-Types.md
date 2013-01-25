Zanata's clients require a project type to be specified. The project type is used in `push` and `pull` operations to help determine where to look for source and translation files, the type of files to look for, and how to deal with the files that are found.

Project type can be specified on the command line, in zanata.xml, in pom.xml, and can now be stored on the server.

Valid values for project type are:
 * Gettext
 * Podir
 * Properties
 * Utf8Properties
 * Xliff
 * Xml
 * Raw - this is an experimental project type that provides limited support for plain-text, LibreOffice and DTD files. Source files must be under a separate directory to translation files.
