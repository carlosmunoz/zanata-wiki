Zanata's clients require a project type to be specified. The project type is used in `push` and `pull` operations to help determine where to look for source and translation files, the type of files to look for, and how to deal with the files that are found.

Project type can be specified on the command line, in `zanata.xml`, in `pom.xml`, and can now be stored on the server.

## Which Project Type?
Valid values for project type are:
 * Gettext - uses gettext format with a single template (.pot) file. Translation files (.po) are named with the locale identifier.
 * Podir - uses gettext format with multiple template (.pot) files. Translation files use the same name as template files, but are placed in a directory named with the locale identifier. Use this type for publican/docbook projects.
 * Properties - handles normal java properties files using ISO-8859-1 encoding (Latin-1). Java properties files require non Latin-1 characters to be escaped with unicode escape characters (e.g. \uFEDC).
 * Utf8Properties - handles non-standard java properties files that use UTF-8 encoding, and do not use unicode escape characters.
 * Xliff - (not actively supported)
 * Xml - (not actively supported)
 * File (previously Raw) - an experimental project type that provides limited support for plain-text, LibreOffice and DTD files. Source files must be under a separate directory to translation files. The behaviour of this project type is subject to change without notice while it is in experimental state.
