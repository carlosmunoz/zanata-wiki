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
    public void makeSomethingHappen() throws Exception {
        String errorMessage = "This is bad text";

        MyPage myPage = new BasicWorkFlow()
                .goToHomePage()
                .goToMyPage()
                .enterSomeTextIntoBox();

        assertThat(myPage.getPageErrors()).contains(errorMessage)
                .as("The page error is displayed");
    }
        
}
```
### Description

> @Test(DetailedTest.class)

All tests should have this, or BasicAcceptanceTest for high priority classes. This is used by the categorised suite runners in Jenkins (e.g. BasicAcceptance for pull requests, Detailed for nightly builds)

> public class MyTest extends ZanataTestCase {

Extend from the base test class to gain test timeouts and detailed reporting.<br><p/>

> @Rule

There are a number of rules that can be used to set up data and control the tests. (See [Test Rules](#test-rules))<br><p/>

> @Test

Defines the function as a test case. Will not be executed without it.<br><p/>

> public void makeSomethingHappen() throws Exception {

A test is a public void function. Adding "throws Exception" allows the test framework to collect it as a test failure, rather than a test error.

> MyPage myPage = new BasicWorkFlow()

Creates a starting point for the test case. The object MyPage is one defined under org.zanata.page.* packages and will hold the elements and functions for interacting with a page.<br>
A workflow defines a set of steps for working with Zanata, such as `LoginWorkFlow()signInAs(...)`, leaving the test on a page for testing or further steps.<p/>

>       .goToHomePage()
>       .goToMyPage()
>       .enterSomeTextIntoBox();

Execute a number of discrete steps resulting in either:<br>
* A page ready for testing data or entities
* A quantifiable value that can be evaluated, wrapped in an assertion<p/>

>        assertThat(myPage.getPageErrors()).contains(errorMessage)
>                .as("The page error is displayed");

The assertion should be in the form assert the `target` has `expected <> condition`, as `human readable expectation` eg.
* `assertThat(page.listOfUsers()).contains(user).as("The user shows in the list")`
* `assertThat(page.isInheritCheckboxChecked()).isTrue().as("The inherit checkbox is checked")`
* As part of a test precondition
```
    assertThat(new LoginWorkFlow()
        .signInAs("admin", "admin")
        .loggedInAs("admin"))
        .isEqualTo("admin")
        .as("Admin has logged in");
    // Rest of test...
```

***

## Test Rules

`SampleProjectRule` Sets up a database with the user set and a test project with data.<br>
`AddUsersRule` Adds the users only, for test that need their own data or the users only.<br>
`CleanDocumentStorageRule` A rule for clearing out the document storage locations.<br>
`HasEmailRule` For checking email composition and sending, eg. register emails.<br>
`RetryRule` For giving tests a "second chance" on failure.

***

## Test Plan and Suites
Test suites are the collections of tests for packages and other suites, and categorised execution of said suite. That is:
* A top level suite - TestPlan - containing a list of the `*Test.java` files in the functional test module and documentation for test types
* BasicAcceptanceTestSuite, DetailedTestSuite etc extend the TestPlan and interface to the @Category annotations

The choice for a flat file, rather than a set of package level suites, stems from the difficulty in maintaining links from test to suite to top level suite - too many layers.

### The Test Plan
Simply a list of all the test cases available to Zanata. This can be run directly to execute every test in Zanata functional-test or indirectly via the BasicAcceptance/DetailedTestSuite classes for categorised execution. Tests not listed here will be ignored by Zanata CI.

```
@RunWith(Suite.class)
@Suite.SuiteClasses({
        MyFeatureTest.class,
        ...
})
public class TestPlan {
}
```

### Category Test Interfaces
These utilise the `@Category` annotation in tests to run a subset of the functional test suite. For example, the `BasicAcceptanceTestSuite` class is used by the GitHub merge test rules to run the high priority tests only.<br>
These do not need to be altered by simple test development - only if new test categories are introduced.

***

## Data Driven Tests
The Zanata test suite uses Theories for executing data driven test. The least subtle difference between this and Paramaterized is that Theories runs by its definition - all points are true else the Theory is false - so the tests will halt on the first encountered failure. Parameterized will execute all data points regardless of result.<br>
The choice for Theories, other than ease of use, is time - don't spend an unnecessary amount of time executing tests that could all be failing for the same reason (e.g someone changed the error string).<br>
### Example
```
/*... licence, package, standard imports ...*/
import org.junit.experimental.theories.DataPoint;
import org.junit.experimental.theories.Theories;
import org.junit.experimental.theories.Theory;
import org.junit.runner.RunWith;

/**
 * @author Me <a href="mailto:me@example.com">me@example.com</a>
 */
@Category(DetailedTest.class)
@RunWith(Theories.class)
public class BadUsernameTest extends ZanataTestCase {

    @DataPoint
    public static String USERNAME_SPACE = "john citizen";

    @DataPoint
    public static String USERNAME_ASTERISK = "**johnny**";

    @DataPoint
    public static String USERNAME_SHORT = "jc";

    @Theory
    public void invalidUsernamesAreRejected(String username) throws Exception {
        String errorMessage = "May only contain 3-20 alphanumeric and underscore characters";

        MyRegisterPage myRegisterPage = new BasicWorkFlow()
                .goToHomePage()
                .goToRegisterPage()
                .enterUsername(username)
                .enterPassword("password")
                .enterEmail("test@test.com")
                .clickSubmitExpectFailure();

        assertThat(myRegisterPage.getPageErrors()).contains(errorMessage)
                .as("The user name invalid error is displayed");
    }
        
}
```
This will run the three DataPoints throught the junit @Test extension @Theory and, if any were to fail, stop immediately.