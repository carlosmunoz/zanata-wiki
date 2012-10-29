# Raw Document Upload and Download

In 2.0 an _experimental_ new feature has been added to enable upload and download of some new types of documents.

TODO link to other pages
The new feature adds upload and download of documents through the website, the REST interface, and with the Maven Plugin.

## Supported Formats
Supported formats include:
* plain-text (*.txt)
* Document Type Definition (*.dtd)
* Open Document Format (LibreOffice):
  * Open Document Text (*.odt, *.fodt)
  * Open Document Presentation (*.odp, *.fodp)
  * Open Document Spreadsheet (*.ods, *.fods)
  * Open Document Graphics (*.odg, *.fodg)

_Open Document Formula (*.odf) and Open Document Database (*.odb) may also work, but have not been tested._

## Known issues
As this is an experimental feature, there are several issues that should be considered when using this feature:

Issue: For most supported formats, the position of paragraphs is used to identify text flows. If a revised source document is uploaded that has a paragraph added or removed (except if it is at the end of the document), any translations from the position of the paragraph onward will be moved to a different text flow and marked as fuzzy.

Workaround: Upload new versions of a document with a different name, then use Copy-trans or Translation Memory to copy translations to matching text flows.


Issue: For most supported formats, translating offline as po will not work. When uploading a downloaded po file, warnings will be shown since Zanata cannot match the translations to the source text flows.

Workaround: Enter translations in the Zanata web editor, or download and translate the document in the original format.



??? known issue ??? - offline translation as original format will usually work but don't add paragraphs to text documents (see above), and source translations that are still in the document may be treated as approved translations.
   recommendation is to translate the document in Zanata, or fully translate the document before upload.


## Instructions for Web Interface
The following instructions assume you have signed in to the Zanata website and have appropriate permissions for the relevant project or locale. If this is not the case, you will not see the buttons or icons referred to in the instructions.
 
### Uploading Source through Web Interface
Source documents are uploaded from the new 'Source Documents' page. Navigate to a project-version and look on the right under 'Actions' for the button labeled 'Source Documents'.

There are 2 methods to upload a source document. To upload a new document, use the 'Upload Document' button under 'Actions' on the right. To upload a new version of an existing document, locate the document in the summary table and click the 'Upload' link in the right-hand column.

The dialogue for uploading a source document is similar for both upload methods, with an additional text box for 'path' when uploading a new document. Select the document to be uploaded, ensuring that it has the appropriate file extension as shown in Supported Formats above. For new documents, enter a path or leave the path field blank to have the document at the top level of the project in Zanata. If the document name and path are the same as an existing document in the table, the uploaded document will replace the existing document.
The uploaded file name is only used for new documents, and will be ignored when replacing an existing document.

### Downloading Source through Web Interface
Source documents can be downloaded from the same page on which they are uploaded (the Source Documents page, see previous section). Look in the 'Download' column in the document table for a link labeled with the raw document file extension. All documents have a .pot download link, but documents with raw data will also have another link indicating the original document type (in this case .odt).


### Uploading Translations through Web Interface


### Downloading Translations through Web Interface


## Instructions for Maven Client
### Uploading with Maven Client
### Downloading with Maven Client



Note about inline tags like <g> etc. (basically leave them as they are).
Note about images, spreadsheet components, etc. that may be confusing.

preview with fuzzies: note that link is not yet provided. Copy URL from download page and change "baked" to "half-baked".