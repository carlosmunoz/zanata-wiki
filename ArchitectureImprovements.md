# Discussion of changes to the architecture of Flies.

# Steps

- create a flies.services layer
- move all business logic out of flies.rest.services and out of webtrans.server.rpc (GWT ActionHandlers) into flies.services or flies.dao (where appropriate)

- move flies.model (domain objects) to separate project (used by clients and server), possibly flies-common-api?
- add JAXB annotations to flies.model and deprecate the parallel DTO hierarchy
- server will use domain objects as persisted objects
- clients will use them as DTOs (ignoring the hibernate annotations)
- new clients will use flies.model instead of flies.dto
- old clients can be migrated incrementally