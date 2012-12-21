Zanata's project pages use Seam components, JSF and RichFaces.

e.g. project/list
built from xhtml page: ```/zanata-war/src/main/webapp/project/home.xhtml```

# URL Rewriting
Each project page is defined as an xhtml page, but is mapped to a URL that does not end with ```.xhtml```. To figure out which xhtml page maps to a particular page on the website, find the rewrite rule in ```urlrewrite.xml``` as follows:

e.g. for the project list on zanata.org, the URL is ```https://translate.zanata.org/zanata/project/list```. Remove the server prefix (```https://translate.zanata.org/zanata```) leaving the local path of the page, ```project/list```. Search for the local path in ```urlrewrite.xml``` (```/zanata-war/src/main/webapp/WEB-INF/urlrewrite.xml```) to find the rule:

```xml
  <rule>
    <from casesensitive="true">^/project/list(.+)?$</from>
    <to last="true">/project/home.seam</to>
  </rule>
```

This rule is taking the ```/project/list``` path and directing it to ```/project/home.seam```. Seam replaces the file extension of each page with ```.seam```, so ```/project/home.seam``` correlates to ```/project/home.xhtml```. Project pages are located in the "zanata" repository (See [[Repositories]]) under ```/zanata-war/src/main/webapp/```, so the page is found at ```/zanata-war/src/main/webapp/project/home.xhtml```.

# Template
Zanata's project pages are based on a template found at ```/zanata-war/src/main/webapp/WEB-INF/layout/template.xhtml```. The template is included near the top of each document in the ```ui:composition``` element, as shown here:

```xml
  <ui:composition ... template="../WEB-INF/layout/template.xhtml">
```


# Page flow and extra parameters (pages.xml)
In Seam, page transitions, parameters, etc. are defined in ```pages.xml```

e.g. for the project list
Search "project/home" in ```/zanata-war/src/main/webapp/WEB-INF/pages.xml```

```xml
  <page view-id="/project/home.xhtml">
    <action execute="#{breadcrumbs.clear}"/>
    <action execute="#{breadcrumbs.addLocation('', 'Projects')}"/>
  </page>
```

This example page is fairly simple, with just the breadcrumbs location defined in ```pages.xml```.


# Seam Components and EL
Seam components are Java classes that can be referenced in Seam pages using Expression Language (EL). A few usages of EL to refer to Seam Components are shown in the following example:

Continuing with the project list example, we can find the code in ```/project/home.xhtml``` that specifies that the project list table should be rendered and where to retrieve the data:

```xml
  <rich:dataTable id="projectList" 
                  rows="#{projectAction.pageSize}"
                  value="#{projectAction.projectPagedListDataModel}"
                  var="project">
      <rich:column width="270px" sortBy="#{project.name}">
          <f:facet name="header">#{messages['jsf.ProjectName']}</f:facet>
          <s:link id="project" styleClass="table_link"
                  value="#{project.name}" propagation="none"
                  view="/project/project.xhtml"
                  rendered="#{project.status == 'ACTIVE'}"> 
              <f:param name="slug" value="#{project.slug}"/>
          </s:link>
          ...
      </rich:column>
      <rich:column width="270px">
          <f:facet name="header">#{messages['jsf.Description']}</f:facet>
          <h:outputText value="#{project.description}" />
      </rich:column>
      ...
  </rich:dataTable> 
```

Notice expressions such as `#{projectAction.pageSize}`, `#{project.name}` and `#{messages['jsf.Description']}` which all use EL and refer to seam components. These EL expressions give the return value from ProjectAction.getPageSize(), the return value of HProject.getName() for each project shown on the page, and a text string from ```zanata-war/src/main/resources/messages.properties```, respectively.

Seam components are Java classes that have a component name defined by a ```@Name``` annotation.

e.g. the ```projectAction``` seam component is defined in ```/zanata-war/src/main/java/org/zanata/action/ProjectAction.java``` by the annotation ```@Name("projectAction")```

```java
  package org.zanata.action;

  import org.jboss.seam.annotations.Name;
  ...

  @Name("projectAction")
  @Scope(ScopeType.PAGE)
  public class ProjectAction implements Serializable
  {
     ...
```

In Zanata, most Seam components have similar class and component names, differing only in capitalization of the first letter. There are a few exceptions, but it should always be possible to use text search to find the ```@Name``` annotation on the component.

e.g. to find ```projectAction``` I can search for ```@Name("projectAction")``` as follows:

```bash
  [damason@damason zanata]$ grep '@Name("projectAction")' **/*.java
  zanata-war/src/main/java/org/zanata/action/ProjectAction.java:@Name("projectAction")
  [damason@damason zanata]$ 
```