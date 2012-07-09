# Guide for Integrating with Zanata

# Introduction

The primary means of communicating with Zanata is through a set of RESTful resources. We have also developed client tools which use REST to communicate with Zanata. This include a Python [[Flies Python Library|library]] and [[Flies Python Client|client]], as well as a Maven plugin for Java projects.

# Configuration for build machine

Create the file `~/.config/zanata.ini` like this:

    [servers]
    jboss.url = https://translate.jboss.org/
    jboss.username = your_jboss_username
    jboss.key = your_API_key_from_Zanata_Profile_page

  NB: Your key can be obtained by logging in to [[Zanata]]    (https://translate.jboss.org/), 
  visiting the [[Profile|page]](https://translate.jboss.org/profile/view) and 
  clicking "generate API key" at the bottom.

# The Scripts

https://github.com/zanata/zanata/tree/master/client/zanata-maven-plugin/sample-scripts/etc/scripts

 - Initial Import (run once only, when first integrating with Zanata)

This script will update the POT (source) and PO (translation) files under `.` from the DocBook XML, and push the content to Zanata for translation.

    etc/scripts/zanata_import_all
    svn add .; svn ci

- Import job (run after documentation freeze)

This script will update the POT files in `pot` and push them to Zanata for translation.  

    etc/scripts/zanata_import_source

- Build docs with latest translations (probably run nightly)

This script will fetch the latest translations to temporary files in `target/draft` and build the documentation for review purposes.  

    etc/scripts/zanata_draft_build
  
- Documentation release build

This script will fetch the latest translations so that the release build can be run.

    etc/scripts/zanata_export_translations
    svn add .; svn ci
    *run existing ant build*
