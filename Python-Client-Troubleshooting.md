# Server version and API version 
When the zanata server deployed is 1.6.1 version, the python client still output "zanata server API version: 1.6.0".
It only means that the API didn't change in Zanata server 1.6.1, the API version used is still 1.6.0.

# Response of Zanata Server

##  301 Moved Permanently
This error occurred when HTTP redirection happened. The user needs to update the server URL to the new URL shown in the warning message.

## 302 Found
Same as 301 error, it is also an error of HTTP redirection.

## 400 Bad Request
The request cannot be fulfilled due to bad syntax of JSON object sending to server -this usually indicates a bug in the Zanata client
 
## 401 Unauthorized 
The operation is not authorized, The user need to check user name and apikey used for Zanata server. Normally, the username and apikey are located in zanata.ini. Or the user can specify the username and apikey with options --username and --apikey

## 403 Forbidden
The user do not have permission to access the resources on the Zanata server. please log on to the server, and check that the language is enabled and check if you have join the language group.

## 404 Not Found
The requested resource is not available on server

## 405 Method Not Allowed 
The requested method is not allowed on server

## 409 Error
The resource with same name already exists on server side. 

## 500 Internal Server Error
An internal error has happened on the Zanata server; please notify the server admin to check the log. 
  1. The version 1.3.5 of zanata-python-client have a bug in query parameters, when use it against zanata server 1.6.1 and 1.7.0. the client will report 500 error. please add an option --noskeleton as a workaround. This issue is fixed in version 1.3.7 and later version.
  2. When commit a big glossary file to zanata server 1.6.x, the server will transaction timeout and return a 500 error to user, this issue is fixed both in 1.7.0 of zanata and latest zanata-python-client (1.3.8)

## 503 Service Unavailable
The service is temporarily unavailable on the server.  Please try the operation again shortly.

# Errors of third part library 

## Errors of python-httplib2 
### SSL certificates error
httplib2 performs verification of SSL certificates as of version 0.7.x and thus often https connections that used to work are now failing because of a failure in verifying certificates.
  1. the python client have add a option in version 1.3.7 to disable verification of certificates: --disable-ssl-cert
  2. python-httplib2 0.7.4-4 have fixed issue with https://translate.zanata.org, please update the python-httplib2 to 0.7.4-4
  3. For internal server, if needed, please add certificates to /usr/lib/python2.7/site-packages/httplib2/cacerts.txt

Reference:
* 0.7.x can't verify wildcard certificates: http://code.google.com/p/httplib2/issues/detail?id=202
* Failed to retrieve the certificate if it has 'subjectAltName' but no 'dns': http://code.google.com/p/httplib2/issues/detail?id=208

### MemoryError
This happens when processing a big file, python-httplib2 will report this error when loading the big file to memory. The python client in latest version(version 1.3.8) have changed to use StringIO to processing big file.   

## Errors of python-polib
### Syntax error
python-polib will check the format of po file. 
  1. `^M` is DOS line break character which shows up in UNIX files when uploaded from a windows file system. It is not allowed in po file
  2. Obsolete previous msgid `#~|`:
  python-polib will treat it as syntax error. This issue is addressed in python-polib 1.0, the python-polib will ignore obsolete previous msgid in po file. please take reference from https://bitbucket.org/izi/polib/issue/28/ioerror-on-reading-obsolete-previous-msgid

## Other errors and warnings 
* The URL mismatch between zanata.xml and zanata.ini

  The URL in zanata.xml and zanata.ini have to strict identical, otherwise, the zanata-python-client will warn user to specify the username and apikey in zanata.ini or by options, since the python-client use the URL to retrieve the username and apikey. This error message is a bit of confusing sometime, so please check the URL carefully. The python client will also revise the error later. 