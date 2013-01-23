This example demonstrates the addition of a simple string property to a persisted entity, including the necessary database update, changes to the entity, and update of JSF pages for input and display of the new property. The property is a URL pointing to a human-readable version of the project source (external to Zanata, such as in the project's version control system), that can be set by a project maintainer and will be displayed on the project's page in Zanata.

This example is of a field that was actually added to the Zanata server. The diff in the Zanata code-base can be viewed on github: [commit: add URL for human-readable source as a new property of HProject, including UI changes ](https://github.com/zanata/zanata/commit/4c4ca9e4d08d4315edb2bf669f0cc01a87eeb705).

## Database Change
Zanata Server uses liquibase to manage database changes. Liquibase uses the concept of a changeset, which describes a database operation. Liquibase runs on server startup, checks a list of changesets against a database table to determine whether there are any changesets yet to run, and runs any that have not yet been run.

In this case, we will be adding a String property named `sourceViewURL` to the entity `HProject`, which will be represented in the database as an extra column on the `HProject` table. The changeset shown below will add the required column:

    zanata-war/src/main/resources/db/changelogs/db.changelog-2.2.xml

    ...
    <changeSet id="1" author="damason@redhat.com">
        <comment>Add source control URL column to HProject.</comment>
        <addColumn tableName="HProject">
            <column name="sourceViewURL" type="longtext">
                <constraints nullable="true" />
            </column>
        </addColumn>
    </changeSet>
    ...

Teaching the detailed structure of a liquibase changeset is beyond the scope of this page. more information can be found in the [Liquibase Quickstart Guide](http://www.liquibase.org/quickstart) and [Liquibase Manual](http://www.liquibase.org/manual/home).


## Entity Change
As well as adding the database column, the property must also be added to the entity, `HProject`. This is a simple change that involves adding a private field and access methods. Because the same name is used for the property, Hibernate will map the property to the database column without the need for any additional configuration.

    zanata-model/src/main/java/org/zanata/model/HProject.java

    @Entity
    @Setter
    ...
    public class HProject extends SlugEntityBase implements Serializable
    {
       ...
       private String sourceViewURL;
       ...
       public String getSourceViewURL()
       {
          return sourceViewURL;
       }
       ...
    }

Note that `setSourceViewURL()` is not added here. This method is added by Lombok because the class `HProject` is annotated with Lombok's `@Setter` annotation.

## Setting the new Property
At this stage, the property has been added to `HProject` and should be properly persisted, but there is no way to set it through the UI. To allow the maintainer of a project to specify the URL, a text entry field will be added to `zanata-war/src/main/webapp/WEB-INF/layout/project_edit_form.xhtml`, which is presented when creating a new project or editing an existing project.

This change looks more complex than it is, due to the use of JSF, Seam Taglib and RichFaces elements and the use of localisable property strings. Essentially this uses a standard property editing template to add the label `Source URL (human-readable)`, an input text field linked to the `sourceViewURL` that was added earlier, and an info icon that displays the text `e.g. https://github.com/zanata/zanata` when moused over.

Translatable strings to display on the UI:

    # zanata-war/src/main/resources/messages.properties
    # Seam handles substitution of these properties in the .xhtml documents where
    # placeholders such as #{messages['jsf.SourceUrlHumanReadable']} are found
    ...
    jsf.SourceUrlHumanReadable=Source URL (human-readable)
    jsf.SourceUrlHumanReadableExample=e.g. https://github.com/zanata/zanata
    ...

Actual UI change:

    zanata-war/src/main/webapp/WEB-INF/layout/project_edit_form.xhtml
    ...
        <s:decorate id="humanViewableSourceUrlField" template="edit.xhtml">
            <ui:define name="label">#{messages['jsf.SourceUrlHumanReadable']}</ui:define>
            <h:inputText id="humanViewableSourceUrl" required="false"
                         value="#{projectHome.instance.sourceViewURL}" style="width:400px;"/>
            <s:span styleClass="icon-info-circle-2 input_help" id="slugHelp">
                <rich:toolTip>
                   <em><code>#{messages['jsf.SourceUrlHumanReadableExample']}</code></em>
                </rich:toolTip>
            </s:span>
        </s:decorate>
    ...

## Displaying the new Property
Now it should be possible for a maintainer to add or update `sourveViewURL` for their project, and to check the current value by looking at the project edit page, but we also want translators to be able to see the URL. A read-only output of the URL will be added on the main page for the project for this purpose. The `rendered` attribute is used to prevent the property being displayed if the maintainer has not set a URL.

Another translatable string is added for the label:

    # zanata-war/src/main/resources/messages.properties
    ...
    jsf.viewSourceFiles=View source files
    ...

And the property display is added: 

    zanata-war/src/main/webapp/project/project.xhtml
    ...
    <p>
        <h:outputText rendered="#{not empty projectHome.instance.sourceViewURL}" value="#{messages['jsf.viewSourceFiles']}: "/>
        <h:outputLink value="#{projectHome.instance.sourceViewURL}">
            <h:outputText value="#{projectHome.instance.sourceViewURL}"/>
        </h:outputLink>
    </p>
    ...


## Adding the Property to the DTO
To make this property accessible over the REST interface, it must also be added to the DTO. This is not yet shown in this example.