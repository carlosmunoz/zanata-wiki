# Raw File Upload/Download REST API

In version 2.0, additional REST methods have been added to handle upload and download of raw documents. This is in addition to the existing API methods, which require documents to be parsed to or generated from an XML or JSON representation client-side.

There are new upload and download methods. The download methods work with a simple GET and return the raw document in full if one is available, or 404 if the requested document has no raw document data on the server. The upload methods work for POST and allow transmission of files byte-for-byte to the server, where they are then parsed. Uploads of large documents can be split over multiple requests to avoid possible timeout issues.

# Download Methods
These will return the raw document byte-for-byte, or 404 if no raw document data is found on the server for the given document id (even if the document id is valid).
## Source Document Download
This will be identical (byte for byte) to the most recently uploaded raw source document at the given location.
`{server address}/rest/file/source/{projectSlug}/{versionSlug}/raw?docId={documentId}`

## Translated Document Download
These methods will return 404 if there is no raw _source_ document data for the given document id, since the raw source data is used to generate the translated raw document.

Two endpoints are available.

The first will use _approved_ translations if they are present, and will keep the source text for any text flow that has no approved translations in the given locale:
`{server address}/rest/file/translation/{projectSlug}/{versionSlug}/{localeId}/baked?docId={documentId}`

The second will use _approved or fuzzy_ translations if they are present, and fall back on source text if they are not:
`{server address}/rest/file/translation/{projectSlug}/{versionSlug}/{localeId}/half-baked?docId={documentId}`


# Upload Methods
Upload endpoints are similar to the download endpoints, but lack the type ('raw', 'baked' or 'half-baked') before the query string. These methods require a multipart form for upload. Examples of usage with curl are shown below, but Zanata's API library provides java classes that can be used in clients to avoid hand-coding parameters this way.

Note that the 'type' parameter must be correct for the document to be parsed properly. A semicolon-separated list of valid types for a server can be obtained from `{server url}/rest/file/accepted_types`.


## Source Document Upload
`{server address}/rest/file/source/{projectSlug}/{versionSlug}?docId={documentId}`

curl command for uploading a source document in the current directory named 'document.txt', where '{upload url}' is the url shown above:

    curl -F type=txt -F file=@document.txt \
    -F hash=`md5sum document.txt | awk '{print }'` \
    -F first=true -F last=true \
    -H "X-Auth-User:{username}" -H "X-Auth-Token:{api key}" \
    "{upload url}"

## Translated Document Upload
`{server address}/rest/file/translation/{projectSlug}/{versionSlug}/{localeId}?docId={documentId}[&merge=import]`

Note the optional '&merge=import' at the end of this URL. DO NOT INCLUDE THIS UNLESS YOU UNDERSTAND EXACTLY WHAT IT DOES. The default merge type 'auto' will use new translations and will avoid using older versions of translations that are in the translation history on the server. Using 'import' will overwrite all existing data on the server for the document.

curl command for uploading a German translation of a document in the current directory named 'document_de.txt', where '{upload url}' is the url shown above:

    curl -F type=txt -F file=@document_de.txt \
    -F hash=`md5sum original_de.txt | awk '{print $1}'`\
    -F first=true -F last=true \
    -H "X-Auth-User:{username}" -H "X-Auth-Token:{api key}" \
    "{upload url}"

## Split Uploads
For large files, uploads can be split over multiple requests. The first part is sent with 'first=true' and 'last=false' and will return an entity that includes an upload id. Subsequent parts are sent with 'first=false' and must include the upload id. The final part should have 'last=true' and will trigger parsing of the complete document on the server.

Note that the hash sent with these requests should be the hash of the complete document, not the hash of each part.

### Split upload example
For this example, suppose I have split a source file 'document.txt' into 3 parts using the following command, giving 'document.txt.aa', 'document.txt.ab' and 'document.txt.ac':
`split -b1024 document.txt document.txt`

I transmit the first part as above, but with 'last=false'. Note that md5sum generates the hash for the _original_ document here, not for this document part:

    curl -F type=txt -F file=@document.txt.aa \
    -F hash=`md5sum document.txt | awk '{print }'` \
    -F first=true -F last=false \
    -H "X-Auth-User:{username}" -H "X-Auth-Token:{api key}" \
    "{upload url}"

The response from the server is as follows, with upload id '12345':

    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <chunkUploadResponse xmlns="http://zanata.org/namespace/api/">
       <acceptedChunks>1</acceptedChunks>
       <expectingMore>true</expectingMore>
       <successMessage>First chunk accepted, awaiting remaining chunks.</successMessage>
       <uploadId>12345</uploadId>
    </chunkUploadResponse>

I transmit the middle part with both first=false and last=false, and include the upload id. This would be repeated if I had split the document into more than three parts (or skipped if I split it into two):

    curl -F type=txt -F file=@document.txt.ab \
    -F hash=`md5sum document.txt | awk '{print }'` \
    -F first=false -F last=false -F uploadId=12345 \
    -H "X-Auth-User:{username}" -H "X-Auth-Token:{api key}" \
    "{upload url}"

The response is identical to the first except for `<acceptedChunks>2</acceptedChunks>`

Finally, I transmit the last part with last=true, which triggers parsing of the document on the server:

    curl -F type=txt -F file=@document.txt.ac \
    -F hash=`md5sum document.txt | awk '{print }'` \
    -F first=false -F last=true -F uploadId=12345 \
    -H "X-Auth-User:{username}" -H "X-Auth-Token:{api key}" \
    "{upload url}"

Assuming no parsing issues with the file, this should return a success response indicating that the document has been added or updated.