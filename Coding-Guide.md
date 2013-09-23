# Coding for the Zanata project

We aim to maintain a consistent coding style for the Zanata project. Keeping style consistent helps keep our code easy to read, helps avoid unnecessary merge conflicts, and ensures that a diff between commits will usually only show what has actually changed. It makes everyone's life a little better.

*Note:* We have recently changed our style preferences. At the time of writing, not all source code has been updated to the new style.

## Copy existing style
The simplest way to use a consistent coding style is to copy the style in the file you are editing, or in nearby files.

## Use an automatic formatter
A set of code formatter preferences can be found in Zanata's source tree, currently located at [zanata-server/zanata-war/eclipse/eclipse-code-formatter-profile.xml](https://github.com/zanata/zanata-server/blob/master/zanata-war/eclipse/eclipse-code-formatter-profile.xml). This can be used by Eclipse and IDEA to automatically format Java code to be consistent with the rest of Zanata code.

## Java Coding Style

```java
public class JavaStyleExample extends Example<Java> implements VeryStylish {

    // Opening curly braces for a class or method are on the same line as the method
    private boolean cuddleBraces = true;

    // Each indentation level is 4 spaces. Do not use tabs for indentation.
    private int indentSpaces = 4;
    private boolean useTabs = false;

    private String longDescription = "TODO";

    @Override
    public String getDescription() {
        return "Java Style Example";
    }

}
```

## XML Coding Style

```xml

```

## JavaScript Coding Style


# General coding standards and style

- See Seam's coding style

# The MVP pattern for GWT

- See the video http://code.google.com/events/io/2009/sessions/GoogleWebToolkitBestPractices.html or the text version at http://extgwt-mvp4g-gae.blogspot.com/2009/10/gwt-app-architecture-best-practices.html
- See the video http://www.youtube.com/watch?v=M5x6E6ze1x8 for GWT 2.1
- Presenter's Display interface methods should generally avoid references to the classes Widget, Button, !TextBox, etc.  Instead, they should use interfaces such as !HasClickHandlers, !HasText, etc
- However, the Display's methods should be meaningful, eg:
    !HasClickHandlers getSubmitButton()

- It should be possible to test the Presenter without instantiating any GWT widget, by using mock implementations of the exposed interfaces.