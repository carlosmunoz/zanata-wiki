# Building a web test case with selenium.

Table of contents

'[[_TOC_]]


# Introduction

[Selenium](http://seleniumhq.org/) is a suite of tools which automates testing of web applications.
Refer http://seleniumhq.org/docs/ for further documentation.


# Glossaries

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

# Prepare the test system
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
  - src/test/java/org/zanata/feature/`featurename`/`FeatureName`TestSuite.java, eg
    `src/test/java/org/zanata/feature/account/AccountTestSuite.java`
  - A list of all other Test Suites, for a full test run or Category filter
- Test Category
  - src/test/java/org/zanata/feature/`CategoryName`TestSuite.java, eg
    `src/test/java/org/zanata/feature/BasicAcceptanceTestSuite.java`
  - Filters by a Test Category name (Interface)
- Test Category Interface
  - src/test/java/org/zanata/feature/`CategoryName`Test.java, eg
    `src/test/java/org/zanata/feature/BasicAcceptanceTest.java`
  - Basically a one liner, but used by the Categories system

# Creating Tests

## JUnit Selenium Tests

Under construction, but:
- Write a test, @Category at top, @Category on BAT's, @Test on functions
- Use Theories, not for loops and if blocks!
- Self contained, does NOT rely on other tests
- Add it to ^TestSuite.java
- Add Test Suite to AggregateTestSuite.java