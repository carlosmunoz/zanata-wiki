These features address an issue that was preventing projects that did not originally use .po format from being translated offline as .po files. Documents could be downloaded as .po but would not work properly during upload (see [[bug 870876 | https://bugzilla.redhat.com/show_bug.cgi?id=870876]]).

To translate offline:
 1. navigate to the project-version of interest, e.g. https://translate.zanata.org/zanata/iteration/view/zanata-server/master?cid=119182
 1. make sure you are signed in
 1. find your language in the table, and click the page icon in the "Documents" column, which goes to something like https://translate.zanata.org/zanata/iteration/files/zanata-server/master/en-GB
 1. At this stage you may see one of a few things:
   - if the new version hasn't deployed yet, there will be no link for config file under "Actions" and the remainder of these instructions are probably not useful
   - if the project type is not set, you should see "Config File" and "Download All Files" under "Actions", but they will be disabled - the project maintainer needs to set the project type before you can continue with these instructions
   - otherwise, the Config File and "Download All Files" links should be enabled
 1. If the links are enabled, choose one of the following:-
   - If you want to translate a single file, use the .po link for that file in the table (this will be disabled under the same conditions that the config and zip are disabled). After translation, use the "upload" link in the "Actions" column next to the same document.
   - If you want a zip containing a config file and all the documents, see "Zip Download" below.
   - If you want to use one of the clients, see "Download with Client" below.

## Zip Download
 - Use the Download All Files link, save the zip and extract to the desired location.
 - 

## Download with Client




 to download files for translation, click the config file link and save zanata.xml to the directory you will work in. This will work with the Java CLI client or the Maven Plugin.
   - , .