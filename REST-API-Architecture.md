The REST API uses RESTEasy and other libraries, and shares some components with the Maven Plugin and Command-Line Client.

Each REST endpoint is mapped to one or more methods in a Java class. This page will use the project endpoint as an example.

# Resource Interfaces

Each endpoint usually has an interface that describes the REST methods that are available at that endpoint, but some interfaces describe methods for multiple related endpoints. These interfaces are located in repository `zanata-api`, project `zanata-common-api` under `/src/main/java/org/zanata/rest/service` and are named `*Resource` (e.g. `ProjectResource`)

Interface methods are annotated with a HTTP method type (e.g. `@HEAD`, `@GET`, `@PUT`). These annotations are found in the `javax.ws.rs` package.

# Server-Side Implementation

Implementation on the server (usually *Service) in zanata under
/zanata-war/src/main/java/org/zanata/rest/service

Client-side resource interface, usually I*Resource in zanata-api-master /zanata-common-api/src/main/java/org/zanata/rest/client which is used by clients via ZanataProxyFactory to easily refer to REST endpoints in client code.

e.g. org.zanata.rest.client.ZanataProxyFactory.getProject(String proj)
