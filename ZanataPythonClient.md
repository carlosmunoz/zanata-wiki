# Python client for working with Zanata

# Introduction

`zanata` is a Python-based command for working with remote zanata servers. This command provides a set of sub-commands for creating a project or iteration, retrieving info on projects, a single project or single iteration. It also provides sub commands for publican  support to pull or push the publican files to/from the Zanata server.

# Getting the Code

The source code is available from [# Install

You can install the python client with `yum install flies-python-client`, or you can download the rpm package from http://jamesni.fedorapeople.org/zanata-python-client/ and install by yourself.

Currently, the external dependency for the zanata-python-client is a library named python-polib. You need to install this dependency before running the zanata-python-client.

# Configuration

After you install the zanata-python-client, you need to create a global configuration file `$USER_HOME/.config/zanata.ini` that contain user-specific configuration. 
python-client uses the same configuration files as the zanata maven client, for details see http://code.google.com/p/flies/wiki/ClientConfiguration. You can manually specify the location of user configuration file with command line option '--user-config', 

For each project, you also have to write a project-specific configuration file named zanata.xml, which is the same zanata.xml used by the zanata maven client, for details see http://code.google.com/p/flies/wiki/ClientConfiguration. You can set the path of project configuration file with command line option '--project-config'.

You can also specify the URL of the zanata server with the command line opition '--url'. There are also command line options '--username' for user name and '--apikey' for api key of user.

# Use Cases

*Name:* `/usr/bin/zanata`

Type "zanata" in the shell, it will give you basic information on available commands for working with the Zanata server. You can use "zanata --help" to get more detailed information about the available commands. 

Basic Usage:
zanata COMMAND `[ARGS](https://github.com/zanata/zanata-python-client])` `[COMMAND_OPTIONS]`

## Listing / Querying for Projects

    $ zanata list

## Creating a project

    $ zanata project create {project_id} --project-name={projec_name} --project-desc={project_description}

### Delete a Project (Not Implemented)

    $ zanata project remove {project_id}

## Working within a single container (i.e. project iteration)

These commands would take settings from the local {{{zanata.xml}}} or you could use the {{{--project}}} and {{{--project-version}}} parameters.

### Create a Project Version

    $ zanata version create {version_id} --project-id={project_id} --version-name={version_name} --version-desc={version_description}

### Delete a Project Version (Not Implemented)

    $ zanata version remove {version_id} --project-id={project_id}

### Query for information of a project

    $ zanata project info --project-id={project_id} 

### Query for information of a project iteration

    $ zanata version info --project-id={project_id} --project-version={version_id} 

### Query for translation status of documents (Not Implemented)

    $ zanata status --project={project_id} --iteration={iteration_id} --lang=[all]|lang1,lang2,.. [documentName]
    /my/document.po
    French (France)  23% translated. 324 words remaining.
    German (Germany) 34% translated. 203 words remaining.
    
    (..repeated for each document...)
    
    Summary:
    French (France)  23% translated. 32423 words remaining.
    German (Germany) 34% translated. 20323 words remaining.

### Publishing Templates to Zanata

If you want to push only one template file to the zanata server, you can use the following command:

    $ zanata publican push --project-id={project_id} --project-version={iteration_id} [documentName..]

If you do not specify a documentName, 'publican push' command will push all the template files under the template folder to the Zanata server. You can specify the path of the template folder by command line option, or let zanata try to locate the template folder in current path by not specifying this option.

    $ zanata publican push --project-id={project_id} --project-version={iteration_id} --srcDir={template_folder}

    $ zanata publican push --project-id={project_id} --project-version={iteration_id} 

Or if you simply run the following command in a folder containing the template folder and zanata.xml, the command will load all the info from configuration file:

    $ zanata publican push 

You can enable 'importPo' option, then related translation (po files) will be pushed to zanata server at the same time. you need to specify the parent folder that contains all the translations by using 'transDir' command line option. By default, the command will read the language info from project configuration file 'zanata.xml', or you can specify the language that you want to push to the zanata server 

    $ zanata publican push --importPo --tranDir={path of parent folder contains locale folders} --lang=lang1,lang2,..
   
You could also enable the 'copyTrans' option, then the server will try to find the closest equivalent translations from other versions in the same project and copy them to version you specified.

$ zanata publican push --copyTrans

## Retrieving translated Documents from Zanata

If you want to retrieve only one file from the zanata server, you can use the following command:

    $ zanata publican pull --project-id={project_id} --project-version={iteration_id} [documentName..]

If you do not specify a documentName, this command will pull all the documents of a project version on Zanata server to a local output folder. The command will read the language info from project configuration file 'zanata.xml', or you can specify the language that you want to pull from the zanata server as follows  


    $ zanata publican pull --project-id={project_id} --project-version={iteration_id} --lang=lang1,lang2,.. --dstDir={output_folder} 

    $ zanata publican pull --project-id={project_id} --project-version={iteration_id} --lang=lang1,lang2,..

you can also simply run the below command in a folder containing zanata.xml, and the command will load all the info from configuration file:

    $ zanata publican pull

## Publishing(Merge) translations of Documents to Zanata (not recommended)

If you want to push translations of Documents to Zanata, you'd better to user 'zanata publican push' command with importPo setting true.

## Workaround for un-merged publican strings

Since gettext uses the source string as the string id, if the source text changes it will have a different id and will no longer be associated with translations of the old version of the source string. Zanata's translation memory allows translators to quickly copy and review the old translation, but these 'fuzzy' TM matches are not reflected in the project statistics, which can be an issue for translator planning.

A suggested workaround to include the old translations as fuzzy matches against changed source strings is to run msgmerge after pulling all the translations form Zanata, then pushing the merged translations back. Publican update po runs msgmerge implicitly.

The following should work for a publican project:

    $ rm -rf pot/
    $ publican update pot
    $ zanata pull
    $ publican update po
    $ zanata push --push-trans