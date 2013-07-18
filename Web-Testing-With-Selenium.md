1. [Introduction](https://github.com/zanata/zanata-server/wiki/Web-Testing-With-Selenium#introduction)
2. [Glossary](https://github.com/zanata/zanata-server/wiki/Web-Testing-With-Selenium#glossary)
3. [Prepare](https://github.com/zanata/zanata-server/wiki/Web-Testing-With-Selenium#prepare)
  * [Packages](https://github.com/zanata/zanata-server/wiki/Web-Testing-With-Selenium#packages)
4. [Structure](https://github.com/zanata/zanata-server/wiki/Web-Testing-With-Selenium#structure)
  * [File naming convention](https://github.com/zanata/zanata-server/wiki/Web-Testing-With-Selenium#file-naming-convention)
5. [Creating Tests](https://github.com/zanata/zanata-server/wiki/Web-Testing-With-Selenium#creating-tests)
  * [A Basic Test](https://github.com/zanata/zanata-server/wiki/Web-Testing-With-Selenium#a-basic-test)
  * [Test Suites](https://github.com/zanata/zanata-server/wiki/Web-Testing-With-Selenium#test-suites)
  * [Ignoring Tests](https://github.com/zanata/zanata-server/wiki/Web-Testing-With-Selenium#ignoring-tests)
  * [Expected Fail](https://github.com/zanata/zanata-server/wiki/Web-Testing-With-Selenium#expected-fail)
  * [Data Based Testing](https://github.com/zanata/zanata-server/wiki/Web-Testing-With-Selenium#data-based-testing)
  * [Lastly](https://github.com/zanata/zanata-server/wiki/Web-Testing-With-Selenium#lastly)


# Introduction

[Selenium](http://seleniumhq.org/) is a suite of tools which automates testing of web applications.
Refer http://seleniumhq.org/docs/ for further documentation.

# Glossary

- Test Case/Method/Function: An encapsulated test that
  - Indicates an intent
  - Executes some step(s)
  - Verifies a result
- Test Suite: A collection of test cases. This may be on the
  - File level (related features)
  - Package level (related concepts)
  - Category level (related priority)
- Test Root: The directory that contains all the tests, and test category definitions.
  - By default, it is at the subdirectory `functional-tests/src/test/java/org/zanata/`.

# Prepare
Get up and running [here](https://github.com/zanata/zanata-server/wiki/Selenium-Web-Driver-Automated-Tests)

## Packages
- A browser. We support
  - Firefox (flaky as of late)
  - Chrome
  - htmlUnit (to some extent)
- The plugins you like. I suggest
  - firebug or DOM inspector.

# Functional Test Structure
A quick overview of the important stuff.

## File naming convention

This is how tests/suites/categories should be named. These files exist in the functional-tests module, so feel free to view them as an example.

- Test File
  - src/test/java/org/zanata/feature/`featurename`/`TestName`Test.java, eg
    `src/test/java/org/zanata/feature/account/RegisterTest.java`
- Test Suite (Feature)
  - src/test/java/org/zanata/feature/`featurename`/`FeatureName`TestSuite.java, eg
    `src/test/java/org/zanata/feature/account/AccountTestSuite.java`
  - Directly references the Test File names in this package
- Test Suite (Top-Level)
  - `src/test/java/org/zanata/feature/AggregateTestSuite.java`
  - A list of all other Test Suites, for a full test run or Category filter
- Test Category
  - src/test/java/org/zanata/feature/`CategoryName`TestSuite.java, eg
    `src/test/java/org/zanata/feature/BasicAcceptanceTestSuite.java`
  - Filters by a Test Category name (Interface)
- Test Category Interface
  - src/test/java/org/zanata/feature/`CategoryName`Test.java, eg
    `src/test/java/org/zanata/feature/BasicAcceptanceTest.java`
  - Basically a one liner, but used by the Categories system
- Test Page
  - src/main/java/org/zanata/page/`feature`/`Feature`Page.java e.g.
    `src/main/java/org/zanata/page/account/RegisterPage.java`
  - A page that holds the components and functions that are used for testing and interacting with the page, for example `WebElement SaveButton` or `public RegisterPage enterUserName(String username)`

# Creating Tests

## A Basic Test
Here's an example of a simple test file.
```java
package org.zanata.feature.myfeature;

import org.hamcrest.Matchers;
import org.junit.Before;
import org.junit.ClassRule;
import org.junit.Test;
import org.zanata.page.HomePage;
import org.zanata.page.myfeature.MyPage;
import org.zanata.util.ResetDatabaseRule;
import org.zanata.workflow.BasicWorkFlow;

import static org.hamcrest.MatcherAssert.assertThat;

/**
 * @author Me <a href="mailto:me@redhat.com">me@redhat.com</a>
 */
@Category(DetailedTest.class) // Everything in this file will run under the DetailedTest category
public class MyFullTest
{
   // Include the class rule ResetDatabaseRule, to start with a clean environment
   @ClassRule
   public static ResetDatabaseRule resetDatabaseRule = new ResetDatabaseRule();

   private HomePage homePage;

   @Before
   public void before()
   {
      // Start with a new Home Page in every test
      homePage = new BasicWorkFlow().goToHome();
   }

   @Test
   @Category(BasicAcceptanceTest.class) // This @Test will run under both DT and BAT categories
   public void aGoodTestName()
   {
      MyPage myPage = homePage.goToMyPage(); // Go to a specific place
      myPage.enterTextInField("My text").pressSave(); // Perform some discrete actions
      // Assert some condition
      assertThat("My feature works", myPage.someElementText, Matchers.equalTo("Isn't broken"));
   }
}
```
Simple, elegant, easy to read. A Test interacts with a Page (MyPage.java) to perform some actions and verify a result.

## Test Suites
Make sure to add your test to the appropriate suites, or it won't be run!
  * Feature Level Suite (right next to the test file)
```java
package org.zanata.feature.myfeature;

import org.junit.runner.RunWith;
import org.junit.runners.Suite;

@RunWith(Suite.class)
@Suite.SuiteClasses({
      MyFeatureTest.class
})
public class MyFeatureTestSuite
{
}
```
  * Top Level Suite - the AggregateTestSuite.java file
```java
...
@RunWith(Suite.class)
@Suite.SuiteClasses({
      EveryoneElsesTestSuite.class,
      MyFeatureTestSuite.class // Add to the list
})
public class AggregateTestSuite {
}
```

## Ignoring Tests
Sometimes you want to ignore a test in test runs, but you don't want to:
- throw it away
- have it sit forever in a block comment
```java
...
import org.junit.Ignore;
...
   /*
    * Ignored Test
    * This test is ignored because the dynamic Captcha is not yet possible to predict
    */
   @Test
   @Ignore("There's a problem with finishing this test")
   public void brokenTest()
   {
      MyPage myPage = homePage.goToMyPage();
      myPage.enterTextInCaptchaField().pressSave(); // Cannot enter a valid Captcha value
      // Assert some condition
      assertThat("My feature works", myPage.someElementText, Matchers.equalTo("Successful"));
   }
```

## Expected Fail
It's often good to have a test that fails, in order to indicate how a feature _should_ work, but not interrupt test runs. Having a test pass unexpectedly means something was fixed and should be recorded as such, and the test reverted to an expected pass.
```java
   /*
    * Bug test
    * The feature is supposed to show "Everything's ok", but doesn't.
    */
   @Test(expected = AssertionError.class)
   public void bug000001_brokenFeature()
   {
      String notification = "Everything's ok";
      MyPage myPage = homePage.goToMyPage();
      myPage.enterTextInField().pressSomething();
      // Fails this assertion, because the feature is broken
      assertThat("My feature works", myPage.someElementText(), Matchers.equalTo(errorMsg));
   }
}
```

## Data Based Testing
If a test has many possible inputs, say a field has some form of validation, don't use a for loop!
Use the (albeit experimental) Theories package.
```java
...
import org.junit.experimental.theories.DataPoint;
import org.junit.experimental.theories.Theories;
import org.junit.experimental.theories.Theory;
import org.junit.runner.RunWith;
...
@RunWith(Theories.class)
public class invalidInputTest {
...
   @DataPoint public static String INVALID_INPUT_FOR_REASON = "Bad input";
   @DataPoint public static String INVALID_INPUT_FOR_ANOTHER_REASON = "More bad input";

   @Theory
   public void invalidInputRejection(String input)
   {
      String errorMsg = "not a well-formed input";
      MyPage myPage = new BasicWorkFlow().goToMyPage();
      myPage = registerPage.enterTextInField(input).pressSomething();
      assertThat("Invalid input error is shown", registerPage.getErrors(), Matchers.hasItem(errorMsg));
   }
}
```
This will run the same test, using all of the DataPoints in the class. For readability, I suggest having data based tests in their own class, to prevent "pollution" of other test classes (due to the nature of the static DataPoints).

## Lastly
Tests ~~should~~ must be
- Self contained, do NOT rely on the results of other tests
- Free of all condition block, no `for`s, `while`s, `if`s, buts or maybes.