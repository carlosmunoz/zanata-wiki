# Coding for the Flies project

# General coding standards and style

- See Seam's coding style

# The MVP pattern for GWT

- See the video http://code.google.com/events/io/2009/sessions/GoogleWebToolkitBestPractices.html or the text version at http://extgwt-mvp4g-gae.blogspot.com/2009/10/gwt-app-architecture-best-practices.html
- See the video http://www.youtube.com/watch?v=M5x6E6ze1x8 for GWT 2.1
- Presenter's Display interface methods should generally avoid references to the classes Widget, Button, !TextBox, etc.  Instead, they should use interfaces such as !HasClickHandlers, !HasText, etc
- However, the Display's methods should be meaningful, eg:
    !HasClickHandlers getSubmitButton()

- It should be possible to test the Presenter without instantiating any GWT widget, by using mock implementations of the exposed interfaces.