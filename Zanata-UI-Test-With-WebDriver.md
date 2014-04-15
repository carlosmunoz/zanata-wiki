

## TestCase
This will be an ever-evolving example along with our test quality and design decisions / improvements.

### Example test

```
/*...licence...*/
package org.zanata.feature.myfeature;

import org.junit.Before;
import org.junit.experimental.categories.Category;
import org.zanata.feature.DetailedTest;
import org.zanata.feature.ZanataTestCase;
import org.zanata.workflow.BasicWorkFlow;
import org.zanata.page.mypages.MyPage;

import static org.assertj.core.api.Assertions.assertThat;

/**
 * @author Me <a href="mailto:me@example.com">me@example.com</a>
 */
@Category(DetailedTest.class)
public class MyTest extends ZanataTestCase {

    @Rule
    public SampleProjectRule sampleProjectRule = new SampleProjectRule();

    @Before
    public void before() {
        //Do some stuff before test
    }

    @Test
    public void makeSomethingHappen throws Exception {
        String errorMessage = "This is bad text";

        MyPage myPage = new BasicWorkFlow()
                .goToHomePage()
                .goToMyPage()
                .enterSomeTextIntoBox();

        assertThat(errorMessage).isIn(myPage.getPageErrors())
                .as("The page error is displayed");
    }
        
}
```
### Description

`@Test(DetailedTest.class)`<br>
All tests should have this, or BasicAcceptanceTest for high priority classes. This is used by the categorised suite runners in Jenkins (e.g. BasicAcceptance for pull requests, Detailed for nightly builds)
<br>
`public class MyTest extends ZanataTestCase {`<br>
Extend from the base test class to gain test timeouts and detailed reporting.<br>
`@Rule`<br>
There are a number of rules that can be used to set up data and control the tests. (See [Test Rules](#test-rules))<br>
`@Test`<br>
Defines the function as a test case. Will not be executed without it.<br>
`MyPage myPage = new BasicWorkFlow()`<br>
Creates a starting point for the test case. The object MyPage is one defined under org.zanata.page.* packages and will hold the elements and functions for interacting with a page.<br>
A workflow defines a set of steps for working with Zanata, such as `LoginWorkFlow()signInAs(...)`, leaving the test on a page for testing or further steps.<br>
```
       .goToHomePage()
       .goToMyPage()
       .enterSomeTextIntoBox();
```<br>
Execute a number of discrete steps resulting in either:<br>
* A page ready for testing data or entities
* A quantifiable value that can be evaluated, wrapped in an assertion
```
        assertThat(errorMessage).isIn(myPage.getPageErrors())
                .as("The page error is displayed");
```
...
## Test Rules

`SampleProjectRule` Sets up a database with the user set and a test project with data.<br>
`AddUsersRule` Adds the users only, for test that need their own data or the users only.<br>
`CleanDocumentStorageRule` A rule for clearing out the document storage locations.<br>
`HasEmailRule` For checking email composition and sending, eg. register emails.<br>

