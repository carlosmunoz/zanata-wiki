Bugzilla number [844820](https://bugzilla.redhat.com/show_bug.cgi?id=844820)
A few other bugs/features have also been fixed/implemented along:
* 730848 - RFE: Colour highlights of tags should stay visible when editing entry
* 836052 - RFE: Red bars for translations with validation warnings should stay in red when moving to the next row
* 729886 - RFE: Making space in editor visible
* 802187 - Translation Unit Details too noisy in editor

### New UI and back end

We have re-implement the translation editor from front to back to have better user experience and easier maintainability. [View labeled diagram here](http://zanata.org/images/screenshots/editor/2.0_new_editor_labeled.svg)
![new editor UI](http://zanata.org/images/screenshots/editor/2.0_new_editor.png)

#### Changes apart from apparent
* Switching between editor, documents list and project wide search and replace are now located on the left as tabs
* Notification messages, workspace users, users options and validation options are located on the right. When click on one it will expand, and click on the same icon will collapse
* New options:
    * Number of messages for each page in editor is now configurable

    ![page size](http://zanata.org/images/screenshots/editor/2.0_page_size_and_others.png)

    * Show System error option. If checked, when there is an unexpected error happening in your browser, you will be able to see it and this information is very helpful for us to troubleshoot the root cause

    ![system error](http://zanata.org/images/screenshots/editor/2.0_show_system_errors.png)

### Note

We use [Code Mirror](codemirror.net) library to perform syntax highlighting. However the library comes with a "feature":
> refresh()

> If your code does something to change the size of the editor element (window resizes are already listened for), or unhides it, you should probably follow up by calling this method to ensure CodeMirror is still looking as intended.

We have already covered most of the cases where we need to call this method. But it's hard to predict how browser renders page and how user will interact with it. If you see your editor become like following:
* Fuzzy or approved translation shows blank in editor
![distort editor in chrome](http://zanata.org/images/screenshots/editor/2.0_CodeMirror_need_refresh_Chrome.png)
![distort editor in firefox](http://zanata.org/images/screenshots/editor/2.0_CodeMirror_need_refresh_FF1.png)
* No line number appearing at the left of the editor and tiny scrollbar appears on the right
![distort editor in firefox](http://zanata.org/images/screenshots/editor/2.0_CodeMirror_need_refresh_FF.png)
Don't panic, all you need to do is to click the little refresh button at the right top corner (beside navigation buttons). This will restore the editors to normal(**Not to confuse it with page reload**).