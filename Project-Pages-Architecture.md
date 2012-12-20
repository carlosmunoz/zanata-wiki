Zanata's project pages use Seam components, JSF and RichFaces.

e.g. project/list
built from xhtml page: /zanata-war/src/main/webapp/project/home.xhtml

# URL Rewriting
Each project page is defined as an xhtml page, but is mapped to a URL that does not end with .xhtml. To figure out which xhtml page maps to a particular page on the website, find the rewrite rule in urlrewrite.xml as follows:

e.g. for the project list on zanata.org, the URL is "https://translate.zanata.org/zanata/project/list". Remove the server prefix (https://translate.zanata.org/zanata) leaving the local path of the page, "project/list". Search for the local path in urlrewrite.xml to find the rule:

/zanata-war/src/main/webapp/WEB-INF/urlrewrite.xml

    <rule>
      <from casesensitive="true">^/project/list(.+)?$</from>
      <to last="true">/project/home.seam</to>
    </rule>

This rule is taking the "/project/list" path and directing it to "/project/home.seam". Seam replaces the file extension of each page with ".seam", so "/project/home.seam" correlates to "/project/home.xhtml". Project pages are located in the "zanata" repository (See [[Repositories]]) under "/zanata-war/src/main/webapp/", so the page is found at "/zanata-war/src/main/webapp/project/home.xhtml"

# Template
Zanata's project pages are based on a template found at /zanata-war/src/main/webapp/WEB-INF/layout/template.xhtml. The template is included near the top of each document in the ui:composition element, as shown here:

    <ui:composition ... template="../WEB-INF/layout/template.xhtml">

# Page flow and extra parameters (pages.xml)
In Seam, page transitions, parameters, etc. are defined in "pages.xml"

e.g. for the project list
Search "project/home" in "/zanata-war/src/main/webapp/WEB-INF/pages.xml"

    <page view-id="/project/home.xhtml">
      <action execute="#{breadcrumbs.clear}"/>
      <action execute="#{breadcrumbs.addLocation('', 'Projects')}"/>
    </page>

This page is fairly simple, with just the breadcrumbs location defined in pages.xml.