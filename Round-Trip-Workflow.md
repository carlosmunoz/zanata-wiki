# Round trip work-flow

<wiki:toc max_depth="3" />

# Introduction

This documents describes the work-flow and functions which are required for each role.
Ideally, tests (preferably automated ones) should be implemented against each item.

The headings with braces are the functionality that haven't been implemented yet,
and serve as note of feature requests.
The *italic* shows the issues and concerns that needs discussion and clarification.

# Roles

## ANONYMOUS

An anonymous is not actually an user, but a visitor that have not sign in.
Yet, they still been able to do the followings:
### View the List of Projects

- See all unhidden projects.

### View the List of Language Teams

- See all language team.

### Fill in the registration from

Fill in following fields:
- Name
- Email: This will be used to send activation.
- Username:
- All in lower case. (*Are numbers, dot, hyphen, underscore allowed?*)
- Check the pre-existing accounts.
- Password/Confirm: (*Shall we provide advanced features, like reject simple password, max/min expire date?*  Not in Flies: anyone with complex requirements should be using external authentication. -SF)
- CAPTCHA: May need an option to disable this (eg for testing)

One can then reply the confirmation email and obtain a user account.

## USER/TRANSLATOR

Once registration is completed, one can become a user by sign in the system.
This role has the capability to perform normal translation tasks, 
like stating which language is he/she interested in,
which project is he/she going to contribute, and of course,
inputting the translation.

### Sign in and Sign out

- If "Remember me" is off, a user must enter username and password to sign in.
- If "Remember me" is on, a user do not need to sign in, simply visiting pages on flies server.
  (*Issues: timeout? what pages are still require sign in (e.g. change profile)?*)

### Join Language Teams

- A user can join or leave one or multiple language teams.
- By join a language team, a user can read and edit the translation of that language in any projects.
- By leave a language team, a user stop being able to read and edit the translation of that language in all projects.

### Join Projects

A user can join a project by clicking on that project.
(*Should access control list (ACL) takes effect here?*)

### Input with Translation Editor

A user can select a document by clicking on the project.
When in a document, a user can:
- Navigate the entries by using either hotkeys or mouse.
- Edit and save an entries by using either hotkeys or click on other entry.
- Copy the original text to translation.
- Mark an entry as fuzzy/not-fuzzy.
- Move to previous/next fuzzy/untranslated entry by using either hotkeys or mouse.

### Edit Own Profile

- Regenerate API key
- (Resend activation mail when change email)
- (Reset password?)
### (Create/Edit Personal Glossary)

### (Proof reading)

### (Preview result)

### (Submit request for creating a project)

## PROJECT MAINTAINER

A project maintainer is a user who is granted the nearly full privilege of assigned projects,
except to create projects, which need an administrator role to perform.

The project maintainer can perform following:

### Create Version

- Web UI
- Name
- Slug (*Shall we enforce naming convention here?*)
- Description
- Active
- Command Line options:
- Able to assign above as options.

### Edit Version

- Edit Name
- Edit description
- Activate/Deactivate

### Import Translation

- From a maven project
- From a publican project
- From an arbitrary project with po files.
 
### Export Translation

- To a maven project
- To a publican project
- To an arbitrary project with po files.

### (Define supported Languages)

- Add language.
- Remove language.

### (Edit Project access control list (ACL))

- Grant world access (any registered user can contribute)
- Allow list
- Deny list

## ADMINISTRATOR

This is a super user of the system. An administrator can perform all task in the system.
In addition of capability of user and project maintainer, an administrator can also:
 
### Configure Server

- Define Server URL

### Manage Users

- Edit user (Web UI and Command Line )
- Username  (*Consider use other element instead of input, which gives the false idea that it's editable.*)
- Set password
- Member of (*Should **project maintainer** appear here?*)
- admin
- user
- Previous created roles.
- Account enabled/disabled
- Delete user: The deleted users should be disappeared after deletion is confirmed.
   (*Should we allow default users to be deleted?*)

### Manage Roles

- Create roles
- Role: Check pre-existing roles
- Member of:
- admin
- user
- Previous created roles.
- Edit roles
- Member of:
- admin
- user
- Previous created roles.
- Delete roles: The deleted roles should be disappeared after deletion is confirmed.
   (*Should we allow default roles to be deleted?*)

### Manage Languages

- Add new languages
- Edit languages

### Manage Search

- Re-indexing

### (Manage Projects)

- Create Projects
- Web UI
- Name
- Slug (*Duplication detect?*)
- Description
- Homepage Content
- Command line
- Able to assign above parameters as options.
- (Assign Project Maintainers)