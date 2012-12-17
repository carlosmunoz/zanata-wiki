Purpose: locate the code for a component or feature of Zanata.
For repository locations, see [[Repositories]].

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
 <td>xhtml pages that use Seam components. Seam components are referred to like #{componentName.methodName}, which refers to a class ComponentName or with annotation @Name("componentName"). An understanding of Seam is very helpful in understanding this code. See http://www.seamframework.org/ or any introductory Seam book.</td>
</tr>

<tr>
 <td></td>
 <td></td>
 <td></td>
 <td></td>
 <td></td>
</tr>

</table>