# Gotchas of all types

# Introduction

# Flies Server

- Flies should be able to re-use translations in many cases, but doesn't.  (eg: migrating from version 1.0 of a project to version 1.1)

# REST API

## Conceptual

- Not really RESTful in design (not enough [hyperlinks](http://en.wikipedia.org/wiki/HATEOAS))
- XRD is untested and doesn't describe all resources (clients don't use it yet)
- Doesn't provide access to everything it should
- Resources at the document level (eg `TranslationsResourceService.getResource()`) require that doc IDs in URLs be encoded in a non-standard way - incompatible with URI templates and XRD, not to mention ugly!

## Practical

- There is no way of deleting translations for obsolete text flows - putting an empty set of text flow targets only deletes the live translations.  If the obsolete text flows are ever re-instated, their translations will come back too.  In some cases, this might be a problem.

# REST Client

## Java/Maven client

- Uses hard-coded URI knowledge instead of XRD URI templates
- Version changes in lock-step with server - we should have independent versioning of flies-war, flies-common and flies-clients
- When exporting PO/POT files, the Java client should have an option to delete obsolete files from the local filesystem.