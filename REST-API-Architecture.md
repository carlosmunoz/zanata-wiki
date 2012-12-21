The REST API uses RESTEasy and other libraries, and shares some components with the Maven Plugin and Command-Line Client.

Each REST endpoint is mapped to one or more methods in a Java class. This page will use the project endpoint as an example.

# Resource Interfaces and Implementation

Interface with methods that are annotated with a HTTP method type. Annotations are found in the javax.ws.rs package (e.g. @HEAD, @GET, @PUT). In zanata-api-master (zanata-common-api) under /src/main/java/org/zanata/rest/service
Implementation on the server (usually *Service) in zanata under /zanata-war/src/main/java/org/zanata/rest/service

Client-side resource interface, usually I*Resource in zanata-api-master /zanata-common-api/src/main/java/org/zanata/rest/client which is used by clients via ZanataProxyFactory to easily refer to REST endpoints in client code.

e.g. org.zanata.rest.client.ZanataProxyFactory.getProject(String proj)

