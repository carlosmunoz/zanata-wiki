# Core workflow describes sequences of important operations.

# Introduction

Core workflow describes sequences of important operations.

# Project handling

Assume we have a project <code>ProjX</code> to be translated.
ProjX might either be [publican](http://fedoraproject.org/wiki/DocsProject/Publican) documentation,
a Maven project, or a software package with conventional po pot files.

## Create a project

This step should be performed by administrators only.

- Create Project
- via web interface
- Name
- Slug
- Description
- Homepage Content
- *via command line (only supported for testing)*
- Able to assign above parameters as options.

## Create a version

This step should be performed by only administrators or project maintainers.
- Web UI
- Name
- Slug
- Description
- Active
- *Command Line options (only supported for testing)*:
- Able to assign above parameters as options.

## Import contents

This step should be performed by only administrators `[project maintainers, _not yet supported_`](`or)`.
Currently only command line importing is supported.

- From a maven/publican project
- From a publican project
- *From an arbitrary project with po files*.

## Start translating

This step can be performed by any registered user. (Access control list is not considered at the moment.)
Assume the user has already signed in, and joined at least one language team.

1. Click on Project -> ProjX enter the dashboard of ProjX.
1. Click on the language the user wants to translate; this brings up the document list page.
1. Click on a document name to start translating that document.
1. Click on the entry to translate. From here, the user can do the following:
- Enter a translation.
- Copy from the original text.
- ~~Clear previous translation.~~
- Move to other entries by using either hot keys or mouse click, automatically saving the current entry,
       if the content of the entry is changed.
- Save the changes
 

## Export contents

This step can be performed by any registered user who has generated an API key. Currently command line only is supported.
- To a maven/publican project
- To a publican project
- *To an arbitrary project with po files*.

# User handling

## Registration

Registration begins with filling in the following fields:
- Name
- Email: This will be used to send activation email.
- Username:
- All in lower case. (*Are numbers, dot, hyphen, underscore allowed?*)
- Check the pre-existing accounts.
- Password/Confirm:
- CAPTCHA: Thus the tests for registration must be manual *or else we must implement special support for testing (eg option to disable CAPTCHA)*.

User should then receive the activation email, click on the embedded link and receive an activated user account.

## Email confirmation

On Zanata server: After receiving registration application, 
a confirmation mail is sent to user-specified mail account
with the correct activation URL.

On the mailbox of the user: The confirmation mail is received,
with the correct activation URL.

The account should be activated after the user clicks the activation URL.
The user can now sign in the Zanata server.

## Join/Leave Language Team

After signed in, a user can join a language team:
1. Click "Language" tab
1. Click the language team he/she wants to join
1. Click "Join Language Team"

After Join, a user can leave the language team:
1. Click "Language" tab
1. Click the language team he/she wants to leave
1. Click "Leave Language Team"