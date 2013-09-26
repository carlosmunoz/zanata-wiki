We aim to maintain a consistent coding style for the Zanata project. Keeping style consistent helps keep our code easy to read, helps avoid unnecessary merge conflicts, and ensures that a diff between commits will usually only show what has actually changed. It makes everyone's life a little better.

*Note:* We have recently changed our style preferences. At the time of writing, not all source code has been updated to the new style. Please don't reformat existing files: we want to do them all at once to keep the history manageable.

## Copy existing style
The simplest way to use a consistent coding style is to copy the style in the file you are editing, or in nearby files.

## Use an automatic formatter
A set of code formatter preferences can be found in Zanata's source tree, currently located at [zanata-server/zanata-war/eclipse/eclipse-code-formatter-profile.xml](https://github.com/zanata/zanata-server/blob/master/zanata-war/eclipse/eclipse-code-formatter-profile.xml). This can be used by Eclipse and IDEA to automatically format Java code to be consistent with the rest of Zanata code.


## Current Styles

### General
* NO TABS!
* line width: 80 (allows side-by-side diffs) (NB: IDEA formatters use the same length for all file types)

### Java, Groovy code
* 4 space indent
* cuddle braces

### JavaScript code
* 2 space indent
* cuddle braces

### XML/XHTML/CSS
* 2 space indent
* format comments, but don't join lines
* insert whitespace before closing empty end-tags
* don't let IDEA align attributes (Eclipse formatter doesn't have this option)

TODO: find some existing (standard) Eclipse code formatter profiles and link to them.



## Examples
These examples aim to show the major style choices we have settled on for Zanata. Please let us know if there is some syntax that is not shown so we can update the examples.

## Java Code Style Example

```java
package org.zanata;

// We use lombok to reduce boilerplate for getters, setters, etc.
// See http://projectlombok.org/ for an overview.
import lombok.Getter;

public class JavaStyleExample {

    // Opening curly braces for a class or method are on the same line as the method
    private boolean cuddleBraces = true;

    // Each indentation level is 4 spaces.
    private int indentSpaces = 4;

    // Do not use tabs for indentation.
    private boolean useTabs = false;

    @Getter
    private String longDescription = "Lines should not exceed 80 characters. Long strings can be " +
                                     "wrapped as shown here to stay below the limit.";

    @Override
    public String toString() {
        return "Java Style Example, showing " + indentSpaces + "-space indents.";
    }
}
```

## XML Coding Style

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xmlStyleExample xmlns="http://www.example.com/xmlStyleExample">
  <indent tabs="false">2 spaces</indent>

  <!-- Comments may be formatted, but auto-formatter should never
       join comment lines. -->

  <attributes wrap="true" align="false"
    description="Attributes can be wrapped to keep line length at or below 80 characters."
    amount="Always indent wrapped attributes 2 spaces more than their tag."
    note="Do not change indentation to align with attributes on the line above." />

  <emptyTags note="Leave a space before closing an empty tag with '/>'" />

</xmlStyleExample>
```

## JavaScript Coding Style

```javascript
(function TODO (when) {
  return 'I will do this ' + when + '.';
})('later');
```

## CSS Coding Style

For more detail, see [CSS Guidelines](https://github.com/lukebrooker/CSS-Guidelines)

```css
.selector-1,
.selector-2,
.selector-3[type="text"] {
  box-sizing: border-box;
  display: block;
  font-family: helvetica, arial, sans-serif;
  color: #333;
  background: #fff;
  background: linear-gradient(#fff, rgba(0, 0, 0, .8));
}

.selector-a,
.selector-b {
  padding: 1em;
}
```

# The MVP pattern for GWT

- See the video http://code.google.com/events/io/2009/sessions/GoogleWebToolkitBestPractices.html or the text version at http://extgwt-mvp4g-gae.blogspot.com/2009/10/gwt-app-architecture-best-practices.html
- See the video http://www.youtube.com/watch?v=M5x6E6ze1x8 for GWT 2.1
- Presenter's Display interface methods should generally avoid references to the classes Widget, Button, !TextBox, etc.  Instead, they should use interfaces such as !HasClickHandlers, !HasText, etc
- However, the Display's methods should be meaningful, eg:
    !HasClickHandlers getSubmitButton()

- It should be possible to test the Presenter without instantiating any GWT widget, by using mock implementations of the exposed interfaces.