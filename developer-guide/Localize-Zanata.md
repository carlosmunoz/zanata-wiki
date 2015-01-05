Zanata itself can be localized. At the moment it uses two different set of resources. One is used in JSF as plain java properties file. One is used in translation editor written in GWT as UTF8 properties. Zanata does not support multi project type in one single project therefore we need to create two separate projects for translating these two set of resources.

### JSF resources
["Zanata server" project on translate.zanata.org](https://translate.zanata.org/zanata/project/view/zanata-server)

It follows standard java resource properties convention and is very easy to set up.

To push and pull, you need to run zanata command under zanata server root (not under zanata-war).

There is an internal jenkins job "push-source-files" for automatically syncing zanata files nightly.

### GWT resources
["Zanata editor" project on translate.zanata.org](https://translate.zanata.org/zanata/project/view/zanata-editor)

This is a bit more involved to set up to work with Zanata. GWT has its own rules and conventions for properties localization. For more information please check [GWT I18N Dev Guide](http://www.gwtproject.org/doc/latest/DevGuideI18n.html) or [GWT I18N Tutorial](http://www.gwtproject.org/doc/latest/tutorial/i18n.html).

In a nutshell, we use annotated java interfaces in code for resource strings. GWT compiler is able to, base on those interfaces, generate resource properties files for localization. However, unlike standard java resource bundle properties, GWT properties differ from:

1. source properties is called xxx_default.properties whereas in java resource bundle it will be xxx.properties.
 > In fact, you **can not** have a xxx.properties exists in your classpath because GWT compiler will try to reflectively load a class called xxx_ which does not exist. For default locale, GWT will use our annotated java interface not a properties file.

2. GWT properties file is UTF8 encoded not ascii

3. GWT properties files support plural forms. If there is no properties file for a given locale, GWT compiler will generate translation skeleton with default strings in it. If GWT compiler then sees a properties file on classpath, it will complement that existing translation properties with required plural form and comments.
```properties
# 0=numDocs (Plural Count)
# - Default plural form
searchFoundResultsInDocuments=Search found results in {0} documents
# - plural form 'one': Count ends in 1 but not 11
searchFoundResultsInDocuments[one]=
# - plural form 'few': Count ends in 2-4 but not 12-14
searchFoundResultsInDocuments[few]=
```
But for complex plural definition i.e. having multiple @PluralCount in parameters (see org.zanata.webtrans.client.resources.WebTransMessages.showingResultsForProjectWideSearch), it needs to have matching number of plural category in the [] just like how you define it in java interface.
i.e.
```java
    @DefaultMessage("Showing results for search \"{0}\" ({1} text flows in {2} documents)")
    // @formatter:off
    @AlternateMessage({
            "one|one", "Showing results for search \"{0}\" (1 text flow in 1 document)",
            "other|one", "Showing results for search \"{0}\" ({1} text flows in 1 document)"
    })
    // @formatter:on
            String showingResultsForProjectWideSearch(String searchString,
                    @PluralCount int textFlows, @PluralCount int documents);
```
The generated plural form below does not match what we have in interface. i.e. not [one|one] and [few|few]
```properties
# 0=searchString, 1=textFlows (Plural Count), 2=documents (Plural Count)
# - Default plural form
showingResultsForProjectWideSearch=Showing results for search "{0}" ({1} text flows in {2} documents)
# - plural form 'one': Count ends in 1 but not 11
showingResultsForProjectWideSearch[one]=
# - plural form 'few': Count ends in 2-4 but not 12-14
showingResultsForProjectWideSearch[few]=
```
It will prevent GWT compiler to compile again with error:
```log
[ERROR] Incorrect number of selector forms for showingResultsForProjectWideSearch - 'few'
```
We can manually fix this (at the moment only 3 such strings). I haven't figured out an automated way to clean this up and Zanata doesn't support mismatch source and target either. So I remove all generated plurals. GWT compiler will give a warning saying missing plural form but it will work.

GWT also use different fall back rule then normal java properties localization. If in GWT module (i.e. Applciatin.gwt.xml) you specified some supported locale(s), i.e. ja, the properties file XXX_ja.properties must be available and compelete at compile time(so that GWT can generate the permutation). It will only fall back to English if user requested locale is NOT listed in gwt.xml. Therefore even if the translation is not 100% complete, you ought to have English strings as placeholders in the translation file. GWT generate skeleton contains all the English placeholder. Currently we have a script to combine Zanata translation and the skeleton to make sure it won't break the build.

#### Steps to generate GWT properties file and push pull:

1. cd into zanata-war

2. mvn zanata:push
 > This has command hook to trigger profile gwt-i18n and ask GWT compiler to generate resource bundle for us. It will also rename and move those properties files into correct location. Enabled locales will have translation skeleton. This is to make sure translation files are pre-filled with contents which is necessary for GWT compiler to compile later on.

3. mvn zanata:pull
 > This will again has command hook to trigger profile gwt-i18n to generate resource skeletons before pull. After pull there is another command hook to combine pulled translation (may not be 100% complete) and generated skeleton file and put it under target/classes. Subsequent build without "clean" will see these files on classpath.

#### Things to note
* At the moment only a few locales are enabled on Zanata server. Those are with high percentage translation rate in ["Zanata server" project](https://translate.zanata.org/zanata/project/view/zanata-server). This is configured in faces-config.xml. GWT follows the same. To enable more locales:

    * edit faces-config.xml
    * add new locale in Application.gwt.xml
    * add new locale in ApplicationI18N.gwt.xml
    * re-run above steps to generate new properties skeleton and push and pull