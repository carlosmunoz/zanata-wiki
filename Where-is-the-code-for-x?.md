Purpose: locate the code for a component or feature of Zanata.

# Project Locations
Zanata has a number of projects (in the Eclipse IDE sense) located in several repositories. Use this table to find which repository a project is in. For repository locations, see [[Repositories]].
<table>
 <tr>
  <th>Repository</th>
  <th>Projects</th>
 </tr>
 <tr>
  <td>zanata-parent</td>
  <td>no projects, parent pom for other projects</td>
 </tr>
 <tr>
  <td>zanata-api</td>
  <td>zanata-common-api</td>
 </tr>
 <tr>
  <td rowspan="5">zanata-common</td>
  <td>zanata-adapter-glossary</td>
 </tr>
 <tr><td>zanata-adapter-po</td></tr>
 <tr><td>zanata-adapter-properties</td></tr>
 <tr><td>zanata-adapter-xliff</td></tr>
 <tr><td>zanata-common-util</td></tr>
 <tr>
  <td rowspan="3">zanata</td>
  <td>functional-test</td>
 </tr>
 <tr><td>zanata-model</td></tr>
 <tr><td>zanata-war</td></tr>
 <tr>
  <td rowspan="5">zanata-client</td>
  <td>zanata-client-ant-po</td>
 </tr>
 <tr><td>zanata-client-ant-properties</td></tr>
 <tr><td>zanata-client-commands</td></tr>
 <tr><td>zanata-maven-plugin</td></tr>
 <tr><td>zanata-rest-client</td></tr>
</table>


# Location of Code for Selected Features
Use this table to find the main packages containing code for the feature of interest. Package prefix "org.zanata" is shortened to "o.z" for brevity.

<table>

<tr>
 <th>Feature</th>
 <th>Project</th>
 <th>Package(s)</th>
 <th>Notes</th>
</tr>

<tr>
 <td>Webtrans (translation editor)</td>
 <td>zanata-war</td>
 <td>o.z.webtrans</td>
 <td>Uses [[GWT|https://developers.google.com/web-toolkit/]].</td>
</tr>

<tr>
 <td rowspan="4">REST API methods</td>
 <td rowspan="3">zanata-common-api</td>
 <td>o.z.rest.client</td>
 <td>Client interfaces for REST endpoints.</td>
</tr>
<tr>
 <td>o.z.rest.dto</td>
 <td>Objects transferred by REST methods.</td>
</tr>
<tr>
 <td>o.z.rest.service</td>
 <td>REST method interfaces.</td>
</tr>
<tr>
 <td rowspan="1">zanata</td>
 <td>o.z.rest.service</td>
 <td>Concrete implementations of REST methods.</td>
</tr>

<tr>
 <td>Project Pages</td>
 <td>zanata</td>
 <td>src/main/webapp and various packages</td>
 <td>xhtml pages that use Seam components. Seam components are referred to like #{componentName.methodName}, which refers to a class ComponentName or with annotation @Name("componentName"). An understanding of Seam is very helpful in understanding this code. See [[Seam Framework|http://www.seamframework.org/]] or any introductory Seam book.</td>
</tr>

<tr>
 <td rowspan="2">Maven Plugin</td>
 <td>zanata-maven-plugin</td>
 <td>o.z.maven</td>
 <td>Bindings of Zanata commands to Maven mojos. See [[Maven - Guide to Developing Java Plugins|http://maven.apache.org/guides/plugin/guide-java-plugin-development.html]]</td>
</tr>
<tr>
 <td>zanata-client-commands</td>
 <td>o.z.client.commands</td>
 <td>Fundamental client command logic. This code performs the main work of the Maven Plugin (and other clients).</td>
</tr>

<tr>
 <td rowspan="4">TM (Translation Memory)</td>
 <td rowspan="4">zanata</td>
 <td>o.z.webtrans.client</td>
 <td>Client-side logic (*.presenter.TransMemoryPresenter) and display (*.view.TransMemoryView)</td>
</tr>
<tr>
 <td>o.z.webtrans.shared.rpc</td>
 <td>Descriptor for gwt-rpc method for fetching TM results.</td>
</tr>
<tr>
 <td>o.z.webtrans.server.rpc</td>
 <td>Concrete implementation for gwt-rpc method for fetching TM results.</td>
</tr>
<tr>
 <td>o.z.dao</td>
 <td>Core logic for retrieving TM matches is found in TextFlowDAO.</td>
</tr>

<tr>
 <td>Validators</td>
 <td>zanata</td>
 <td>o.z.webtrans.shared.validation</td>
 <td></td>
</tr>


</table>