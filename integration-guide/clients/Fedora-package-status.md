#### Dependencies/packages tree that needs to go into Fedora
    zanata-client
      |--zanata-common
         |--zanata-api
         |--opencsv
         |--openprops
         |--jgettext
            |--zanata-parent
**Note** opencsv is already in Fedora but was maintained by a community user. It was built by ant hence missing pom file in Fedora and causing maven build difficulty. Previous owner decided to release ownership of the package and we(pahuang) took it over and change it to build by maven.
#### Status update (as of 1 May 2013)
At the time of writing, there are 4 Fedora platform we need to support: f18, f19, f20, rawhide

---
            zanata-parent   openprops   jgettext    opencsv zanata-api  zanata-common   zanata-client
    f18     +               +           +           +       +           +               +
    f19     +               +           +           +       +           +               + 
    f20     +               +           +           +       +           +                  
    rawhide +               +           +           +       +           +                      
**Notion** +: in stable ?: in testing -: need review

#### Bugzilla for new package review
* zanata-parent - [916023](https://bugzilla.redhat.com/show_bug.cgi?id=916023)      
* openprops     - [908168](https://bugzilla.redhat.com/show_bug.cgi?id=908168)
* jgettext      - [908584](https://bugzilla.redhat.com/show_bug.cgi?id=908584)
* opencsv       - existing package
* zanata-api    - [924513](https://bugzilla.redhat.com/show_bug.cgi?id=924513)
* zanata-common - [958006](https://bugzilla.redhat.com/show_bug.cgi?id=958006)
* zanata-client - [962256](https://bugzilla.redhat.com/show_bug.cgi?id=962256)         