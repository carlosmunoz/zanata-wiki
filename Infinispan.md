# Infinispan

_This section is still under review and is about features that have not been released yet_

Zanata uses Infinispan to manage its internal data caches and search indexes. Configuration for these caches happens in JBoss' `standalone/configuration/standalone.xml`. There are three different caches that need to be configured for Zanata:

1. Hibernate Cache
1. Hibernate search Indexes
1. Other internal data caches

The Infinispan configuration will be located inside the following module in `standalone.xml`:

```xml
<subsystem xmlns="urn:jboss:domain:infinispan:1.4">
...
</subsystem>
```

Keep in mind that the module version may vary depending on your JBoss version.

### Hibernate Cache

The following is the recommended configuration for the Hibernate cache:

```xml
...
<cache-container name="hibernate" default-cache="local-query" jndi-name="java:jboss/infinispan/container/hibernate" start="EAGER" module="org.jboss.as.jpa.hibernate:4">
    <local-cache name="entity">
        <transaction mode="NON_XA"/>
        <eviction strategy="LRU" max-entries="10000"/>
        <expiration max-idle="100000"/>
   	</local-cache>
    <local-cache name="local-query">
        <transaction mode="NONE"/>
        <eviction strategy="LRU" max-entries="10000"/>
        <expiration max-idle="100000"/>
    </local-cache>
    <local-cache name="timestamps">
        <transaction mode="NONE"/>
        <eviction strategy="NONE"/>
    </local-cache>
</cache-container>
...
```

Depending on your JBoss installation, the hibernate cache might already be present in the configuration, in which case there is no need to create another one, but just modify it.

### Hibernate Search indexes

The following is the recommended configuration for the Hibernate search index cache:

```xml
...
<cache-container name="hibernate-search" default-cache="indexes" jndi-name="java:jboss/infinispan/container/hibernate-search" start="EAGER"  module="org.jboss.as.clustering.web.infinispan">
    <local-cache name="LuceneIndexesMetadata" start="EAGER" batching="false">
        <locking isolation="REPEATABLE_READ" striping="false" acquire-timeout="20000" concurrency-level="500"/>
        <transaction mode="NONE"/>
        <eviction strategy="NONE"/>
        <binary-keyed-jdbc-store datasource="java:jboss/datasources/zanataDatasource" passivation="false" purge="false">
            <property name="databaseType">
                MYSQL
            </property>
            <binary-keyed-table prefix="HSI">
                <id-column name="id" type="VARCHAR(255)"/>
                <data-column name="datum" type="LONGBLOB"/>
                <timestamp-column name="version" type="BIGINT(20)"/>
            </binary-keyed-table>
        </binary-keyed-jdbc-store>
    </local-cache>
    <local-cache name="LuceneIndexesData" start="EAGER" batching="false">
        <locking isolation="REPEATABLE_READ" striping="false" acquire-timeout="20000" concurrency-level="500"/>
        <transaction mode="NONE"/>
        <eviction strategy="NONE"/>
        <binary-keyed-jdbc-store datasource="java:jboss/datasources/zanataDatasource" passivation="false" purge="false">
            <property name="databaseType">
                MYSQL
            </property>
            <binary-keyed-table prefix="HSI">
                <id-column name="id" type="VARCHAR(255)"/>
                <data-column name="datum" type="LONGBLOB"/>
                <timestamp-column name="version" type="BIGINT(20)"/>
            </binary-keyed-table>
        </binary-keyed-jdbc-store>
    </local-cache>
    <local-cache name="LuceneIndexesLocking" start="EAGER" batching="false"/>
</cache-container>
...
```

The datasource name can be the same one used in Zanata, but a different one may be selected. In this case, it's assumed it's the same main Zanata datasource.

### Other internal data caches

```xml
...
<cache-container name="zanata" default-cache="default" jndi-name="java:jboss/infinispan/container/zanata" 
                 start="EAGER" 
                 module="org.jboss.as.clustering.web.infinispan">
    <local-cache name="default">
        <transaction mode="NON_XA"/>
        <eviction strategy="LRU" max-entries="10000"/>
        <expiration max-idle="100000"/>
    </local-cache>
</cache-container>
...
```