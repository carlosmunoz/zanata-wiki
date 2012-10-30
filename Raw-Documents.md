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

### Uploading modified versions of existing documents can cause translation misalignment
For most supported formats, the position of paragraphs is used to identify text flows. If a revised source document is uploaded that has a paragraph added or removed (except if it is at the end of the document), any translations from the position of the paragraph onward will be moved to a different text flow and marked as fuzzy.

_**Workaround:** Upload new versions of a document with a different name, then use Copy-trans or Translation Memory to copy translations to matching text flows._

### .po offline translations cannot be uploaded properly
For most supported formats, translating offline as po will not work. When uploading a downloaded po file, warnings will be shown since Zanata cannot match the translations to the source text flows. This is related to a difference in how text flows are identified in different file formats.

_**Workaround:** Enter translations in the Zanata web editor, or download and translate the document in the original format._

### Offline translations in the original format must have exactly the layout of the source document
Offline translation in the original document format will work properly if paragraphs are not rearranged at all. If any paragraphs are added, split or removed, text flows after the affected paragraph will be associated with the wrong source string.

_**Workaround:** Do not change the number of paragraphs or rearrange any paragraphs when doing offline translation. A more reliable solution is to use the web translation editor instead of translating offline._

### Source strings in translated documents will be uploaded as translations
If a translation document is uplaoded that is only partially translated, any source strings that are still in the document will be treated as approved translations.

_**Workaround:** Only upload translation documents that are completely translated. Alternatively, enter translations in the web translation editor to avoid the issue entirely._


## Instructions for Web Interface
The following instructions assume you have signed in to the Zanata website and have appropriate permissions for the relevant project or locale. If this is not the case, you will not see the buttons or icons referred to in the instructions.

For illustration, I will use a sample .odt document and a translated version as shown here:
![Sample document used in examples](http://zanata.org/images/screenshots/raw-files/sample-document.png)
![Sample translated document used in examples](http://zanata.org/images/screenshots/raw-files/sample-document-trans.png)




### Uploading Source through Web Interface
Source documents are uploaded from the new 'Source Documents' page. Navigate to a project-version and look on the right under 'Actions' for the button labeled 'Source Documents'.

![Source Documents button](http://zanata.org/images/screenshots/raw-files/source-documents-button.png)


There are 2 methods to upload a source document. To upload a new document, use the 'Upload Document' button under 'Actions' on the right. To upload a new version of an existing document, locate the document in the summary table and click the 'Upload' link in the right-hand column.

![Source Upload button](http://zanata.org/images/screenshots/raw-files/source-upload-button.png)


The dialogue for uploading a source document is similar for both upload methods, with an additional text box for 'path' when uploading a new document. Select the document to be uploaded, ensuring that it has the appropriate file extension as shown in Supported Formats above. For new documents, enter a path or leave the path field blank to have the document at the top level of the project in Zanata. If the document name and path are the same as an existing document in the table, the uploaded document will replace the existing document.
The uploaded file name is only used for new documents, and will be ignored when replacing an existing document.

![Source Upload dialogue](http://zanata.org/images/screenshots/raw-files/source-upload-dialog.png)


Documents will appear in the document table upon successful upload. Note path matches the entered value, and document name uses the name of the uploaded file:
![Source document in table after successful upload](http://zanata.org/images/screenshots/raw-files/source-upload-success.png)



### Downloading Source through Web Interface
Source documents can be downloaded from the same page on which they are uploaded (the Source Documents page, see previous section). Look in the 'Download' column in the document table for a link labeled with the raw document file extension. All documents have a .pot download link, but documents with raw data will have another link indicating the original document type (in this case .odt).


### Uploading Translations through Web Interface
Translated documents are uploaded from the 'Documents' page for the relevant locale. Navigate to a project-version and click the icon in the 'Documents' column on the row for the locale of choice. Translated documents should be of the same file format as the raw source document, which is reflected in the document name and the raw download link.


![Translated Documents link](http://zanata.org/images/screenshots/raw-files/trans-documents-link.png)

To upload a translated version of a document, locate the document in the summary table and click the 'Upload' icon under the 'Actions' column. A popup is shown with the name of the document at the top and a button to select the translated version of the document. It is recommended to keep the 'Merge?' option selected to avoid losing existing translations.

![Translated Document upload link](http://zanata.org/images/screenshots/raw-files/trans-upload-link.png)

![Translated Document upload dialogue](http://zanata.org/images/screenshots/raw-files/trans-upload-dialog.png)

Upon successful upload, the translation statistics for the document should reflect the addition of the uploaded translations, and the translation strings will be visible in the translation editor:
![Translations visible in translation editor](http://zanata.org/images/screenshots/raw-files/webtrans-uploaded.png)

Please be aware of the current issues with raw translation upload. See 'Known Issues' on this page.

### Downloading Translations through Web Interface
Translated documents can be downloaded from the 'Documents' page for the relevant locale (see previous section). Look in the 'Download' column in the document table for the link labeled with the raw document file extension. All documents have a .po download link, but documents with raw data will have another link indicating the original document type (in this case .odt).

The raw download link will generate a translated document that has the structure of the raw source document, but with source strings replaced with _Approved_ translations for the locale if they are available. For any text flow that does not have an approved translation in Zanata, no substitution is made so the source text will remain.

![Translated Document download link](http://zanata.org/images/screenshots/raw-files/trans-download-link.png)

It is also possible to generate a preview document that has both _Approved_ and _Fuzzy/Needs Review_ translations. Currently there is no link for this 'preview' functionality, so the URL must be modified manually. To do so, copy the normal download link and paste it to the browser address bar, find the part of the URL with '/baked?' and insert 'half-' to make that part '/half-baked?'. Navigate to the modified URL (usually by pressing _Enter_) to download the preview document.

![Modifying Translated Document link to generate a preview document](http://zanata.org/images/screenshots/raw-files/trans-download-preview.png)


## Instructions for Maven Client
To use 'push' and 'pull' commands with the Zanata Maven Plugin, set the project type to 'raw'. See the detailed help for push and pull commands for additional details.

### Uploading with Maven Client
Check Maven Plugin help for 'push' command.

### Downloading with Maven Client
Check Maven Plugin help for 'pull' command.


## Tips for Translating 'Raw' Documents
### Inline Tags
Some parts of raw documents are not intended for direct translation. These are converted to xml-style inline tags such as "<g1><g2></g2></g1>" in the place of the image in the example document. It is recommended that these tags be included in translations with no modifications. The "XML/HTML tags" validator can help detect accidental changes to inline tags. If unsure, you can also download a preview document to ensure that there are no errors or layout problems associated with treatment of tags - see last paragraph of "Downloading Translations through Web Interface"

![XML/HTML validator works for inline tags](http://zanata.org/images/screenshots/raw-files/inline-tags-validation.png)