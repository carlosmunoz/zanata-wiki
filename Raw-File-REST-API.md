# Raw File Upload/Download REST API

In version 2.0, additional REST methods have been added to handle upload and download of raw documents. This is in addition to the existing API methods, which require documents to be parsed to or generated from an XML or JSON representation client-side.

There are new upload and download methods. The download methods work with a simple GET and return the raw document in full if one is available, or 404 if the requested document has no raw document data on the server. The upload methods work for POST and allow transmission of files byte-for-byte to the server, where they are then parsed. Uploads of large documents can be split over multiple requests to avoid possible timeout issues.

# download methods

