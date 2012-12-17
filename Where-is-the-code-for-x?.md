Purpose: locate the code for a component or feature of Zanata.

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


<table>

<tr>
 <th>Feature</th>
 <th>Repository</th>
 <th>Project</th>
 <th>Package(s)</th>
 <th>Notes</th>
</tr>

<tr>
 <td>Webtrans (translation editor)</td>
 <td>zanata</td>
 <td>zanata-war</td>
 <td>org.zanata.webtrans</td>
 <td>Uses [[GWT|https://developers.google.com/web-toolkit/]]</td>
</tr>

<tr>
 <td rowspan="4">REST API methods</td>
 <td rowspan="3">zanata-api</td>
 <td rowspan="3">zanata-common-api</td>
 <td>org.zanata.rest.client</td>
 <td>Client interfaces for REST endpoints</td>
</tr>
<tr>
 <td>org.zanata.rest.dto</td>
 <td>Objects transferred by REST methods</td>
</tr>
<tr>
 <td>org.zanata.rest.service</td>
 <td>REST method interfaces</td>
</tr>
<tr>
 <td rowspan="1">zanata</td>
 <td rowspan="1">zanata-war</td>
 <td>org.zanata.rest.service</td>
 <td>Concrete implementations of REST methods</td>
</tr>

<tr>
 <td>Project Pages</td>
 <td>zanata</td>
 <td>zanata-war</td>
 <td>src/main/webapp and various packages</td>
 <td>xhtml pages that use Seam components. Seam components are referred to like #{componentName.methodName}, which refers to a class ComponentName or with annotation @Name("componentName"). An understanding of Seam is very helpful in understanding this code. See [[Seam Framework|http://www.seamframework.org/]] or any introductory Seam book.</td>
</tr>

<tr>
 <td></td>
 <td></td>
 <td></td>
 <td></td>
 <td></td>
</tr>

</table>