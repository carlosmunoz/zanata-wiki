Zanata uses Hibernate for persistence. Hibernate provides Object-Relational Mapping (ORM), mapping database tables to Java objects and greatly reducing the amount of SQL required for persistence. These objects ("Entities") are used for most server code, but data is converted into Data Transfer Objects (DTOs) before being sent to webtrans or over the REST interface.

# Entities
Hibernate maps Java objects (Entities) to one or more database tables. Classes are given the `@Entity` annotation to allow Hibernate to identify them as entities, as well as numerous other annotations to describe available operations, relationships between entities, field constraints, and so on.

In Zanata, entity class names are prefixed with 'H' (for 'Hibernate'), and are located in repository `zanata` project `zanata-model` under `org.zanata.model`.

e.g. `org.zanata.model.HProject`

# Retrieving Entities
Most accessing of entities in Zanata is done through Data Access Objects (DAOs), which wrap Hibernate and SQL queries to provide simple methods for retrieving entities. DAOs in Zanata are suffixed with 'DAO' and are located in repository `zanata` project `zanata-war` under `org.zanata.dao`.

e.g. `org.zanata.dao.ProjectDAO`

# Data Transfer Objects
Entities are used in server-side code, but are not ideal for network transmission for several reasons that are not address on this page. Before transmission for REST or GWT-RPC requests, the data from an entity is transferred to a Data Transfer Object (DTO). Transfer between DTOs and entities is handled in various services, e.g. `ProjectService.toResource(...)`.

DTOs are located in repository `zanata-api` project `zanata-common-api` under `org.zanata.rest.dto` and sub-packages, and they usually do not have any prefix/suffix.

e.g. `org.zanata.rest.dto.Project`
