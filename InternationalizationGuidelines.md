# Everything about I18N.

# Introduction

The purpose of this document is to provide readers brief understandings on workflow of internationalization for Zanata Project.

The readers are expected to have general knowledge of internationalization about JBoss Seam, Google Web Toolkit (GWT), Java Server Faces (JSF) and Java. It might requires the readers to read further on relevant documentations that are mostly available on the Internet.

After reading this document, the reader are expected to be able to internationalize Zanata and make it ready for localization process.

# Details

There are two main parts in the Flies for internationalization - UI of GWT and JSF.

## GWT

Here are the steps of internationalization on Flies (GWT):

- Read [GWT Internationalization Documentation](http://code.google.com/webtoolkit/doc/latest/DevGuideI18n.html) thoughtfully.
- Set up the list of locales to be supported by adding the followings in package "org.fedorahosted.flies.webtrans"/Application.gwt.xml, between <module> </module>. This is just an example, you can add more locale by changing value in extend-property tag:
      <!-- English language, independent of country -->
      <extend-property name="locale" values="en"/>
    
      <!-- Set default locale to be "en" -->
      <set-property name="locale" value="en" />
- Locate all strings specified in a .java file that will be set for the GWT widgets.
- Create a "messages class" in the same package of the .java, which extends com.google.gwt.i18n.client.Messages.
- Create methods in the messages class with annotation @DefaultMessage("Message by dafault").
- Compile GWT by the following command at root (or flies-war/):
    $ mvn gwt:compile
- GWT will generate .properties files automatically at target/extra/.
- Create the correspond packages name in src/resources/ as the .java file and message file located.
- Copy over the automatically generated .properties files to the new pacakges in src/resources/.
- Remove package prefixes of the .properties files. i.e. They will be same name as their message files.
- Do some sample translation and deploy for testing.

## JSF

Here are the steps of internationalization on Zanata (JSF):

- Read [Seam Internationalization Documentation](http://docs.jboss.org/seam/2.2.1.CR1/reference/en-US/html/i18n.html) thoughtfully.
- Before adding a new locale, there is a need to create a widget for user to change locale. Besides, you can still test for setting locale by adding "&locale=en" (or other locales) at the end of the URL input field of your browser during browse of JSF pages.
- Add supported locales to the file: /flies/src/main/webapp/WEB-INF/faces-config.xml, between tag of <faces-config>. Those are the sample locales and you could customize for yours::
       <application>
          <locale-config>
             <default-locale>en</default-locale>
             <supported-locale>bg</supported-locale>
             <supported-locale>de</supported-locale>
             <supported-locale>en</supported-locale>
             <supported-locale>fr</supported-locale>
             <supported-locale>it</supported-locale>
             <supported-locale>tr</supported-locale>
          </locale-config>
          <view-handler>com.sun.facelets.FaceletViewHandler</view-handler>
       </application>
- Open the .xhtml JSF files and locate strings. Sometimes they are between tags, but sometimes they are sitting as values within the tags of buttons or other widgets.
- Replace the strings with `#{messages['stringKey']}`. Pick an unique key for each of the strings.
- Create/modify messages*.properties files in src/resources/. The messages.properties as fallback might not be there, so you could clone from English one messages_en.properties.
- Create translations by adding a line in .properties files:
    stringKey=Translated string.
- Build and deploy to server for testing, from my development environment I run after I have started my application server for deployment:
    $ mvn -Pjboss4,dev,nogwt,explode clean package

# Conclusion

Once GWT and JSF are internationalized, the localization process is ready to commence. The translators will be working on .properties files in the following directories and files::

- JSF UI: src/resources/messages*.properties
- GWT UI: src/resources/`[name of the packages of Java messages files]`

 - **For the .properties files, the key should not be spaced. Otherwise, it will not be linked by JSF files.**

# References

- http://docs.jboss.org/seam/2.2.1.CR1/reference/en-US/html/i18n.html
- http://code.google.com/webtoolkit/doc/latest/DevGuideI18n.html