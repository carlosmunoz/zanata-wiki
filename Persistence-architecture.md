Zanata uses Hibernate for persistence. Hibernate provides Object-Relational Mapping (ORM), mapping database tables to Java objects and greatly reducing the amount of SQL required for persistence. These objects ("Entities") are used for most server code, but data is converted into Data Transfer Objects (DTOs) before being sent to webtrans or over the REST interface.

# Entities
Hibernate maps Java objects (Entities) to one or more database tables. Classes are given the `@Entity` annotation to allow Hibernate to identify them as entities, as well as numerous other annotations to describe available operations, relationships between entities, field constraints, and so on.

In Zanata, entity class names are prefixed with 'H' (for 'Hibernate'), and are located in repository `zanata` project `zanata-model` under `org.zanata.model`.

e.g. `HProject`

# Retrieving Entities
Most accessing of entities in Zanata is done through Data Access Objects (DAOs), which wrap Hibernate and SQL queries to provide simple methods for retrieving entities. DAOs in Zanata are suffixed with 'DAO' and are located in repository `zanata` project `zanata-war` under `org.zanata.dao` and sub-packages.

e.g. `ProjectDAO`

# Data Transfer Objects