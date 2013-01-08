The REST API uses RESTEasy and other libraries, and shares some components with the Maven Plugin and Command-Line Client.

Each REST endpoint is mapped to one or more methods in a Java class. This page will use the project endpoint as an example.

# Resource Interfaces

Each endpoint usually has an interface that describes the REST methods that are available at that endpoint, but some interfaces describe methods for multiple related endpoints. These interfaces are located in repository `zanata-api`, project `zanata-common-api` under `/src/main/java/org/zanata/rest/service` and are named `*Resource` (e.g. `ProjectResource`)

Interface methods are annotated with a HTTP method type (e.g. `@HEAD`, `@GET`, `@PUT`). These annotations are found in the `javax.ws.rs` package.

# Server-Side Implementation

Each Resource Interface has a concrete implementation in the server, responsible for handling the actual REST method calls. Implementations are found in repository `zanata`, project `zanata-war` under `/src/main/java/org/zanata/rest/service` and are named `*Service` (e.g. `ProjectService`).

The URI path for each method (or the entire implementation class) is specified with `@Path` annotations in the implementation. For example, `ProjectService` class is annotated with `@Path(ProjectService.SERVICE_PATH)`, `SERVICE_PATH` is a string which describes a path of `/projects/p/<projectSlug>` from the base server rest path.


# Client-Side Resource Interfaces

Each Resource Interface is extended to make a client-side interface used by RESTEasy to create proxy methods. Client-side interfaces are found in repository `zanata-api`, project `zanata-common-api` under `/src/main/java/org/zanata/rest/client`, and are usually named `I*Resource` (e.g. `IProjectResource`).

Client-side interfaces are used by `ZanataProxyFactory` to create proxy methods used by Zanata clients, e.g. `org.zanata.rest.client.ZanataProxyFactory.getProject(String proj)`. `ZanataProxyFactory` is in repository `zanata-client`, project `zanata-rest-client` under `/src/main/java/org/zanata/rest/client`.
