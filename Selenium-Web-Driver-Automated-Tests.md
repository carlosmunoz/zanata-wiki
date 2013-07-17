You can see examples under functional-test module.

##### What is Selenium Web Driver? 
[Introduction](http://docs.seleniumhq.org/projects/webdriver/)

##### Why do we use it? 
You can find more information [here](http://docs.seleniumhq.org/docs/03_webdriver.jsp). But in short this is a better way to write your test and easier to maintain you test code. And it can integrate well into maven build life cycle.

##### What is Concordion? 
[Acceptance test specification for java](http://www.concordion.org)

##### Why do we use it? 
We want our tests to become our documentation.

### Structure of Zanata server/functional-test module
* src/main/java/org/zanata/page/**/*Page.java
    * all the web driver mapped page objects.
* src/main/java/org/zanata/page/WebDriverFactory.java
    * this is the factor to create a singleton web driver instance to use. It will read in a properties file and based on the value it will create different driver (i.e. firefox, chrome, htmlUnit).
* src/main/java/org/zanata/workflow/*.*.java
    * these mapped to actual Zanata workflow. It encapsulate common page interactions.
* src/test/java/org/zanata/concordion
    * concordion extensions
* src/test/java/org/zanata/feature/*TestSuite.java
    * Top level test suite and Categories
* src/test/java/org/zanata/feature/**/*Test*.java
    * tests run by concordion and junit runners that use web driver to perform our automated tests
* src/test/resources/concordion/org/zanata/feature/**/*.html
    * concordion specifications
* src/test/resources/setup.properties
    * this file is the properties file being used in WebDriverFactory. It will be filtered by maven.
* src/test/resources/zanata-user-config/*.ini
    * zanata config file used by tests. It will be filtered by maven.
* sample-projects
    * sample projects that are used to test zanata client server interaction. zanata.xml and pom.xml will be filtered by maven.
* src/test/resources/
    * everything else under this are files needed to setup a JBoss EAP6(AS7) instance for Zanata. See pom.xml under cargo maven plugin.

### How to run it
Functional tests assume a zanata-{version}.war exists in server/zanata-war/target. So assuming you are at zanata server module top level folder:

    $ls -d */
    functional-test/  zanata-dist/  zanata-model/  zanata-war/
    $mvn clean package -Pchromefirefox -DskipTests
    ... BUILD SUCCESS
    $find zanata-war/target -type f -name "zanata*.war"
    zanata-war/target/zanata-{version}.war

**-Pchromefirefox** builds GWT code to work with both. **-Pfirefox** will only build to work with firefox. **-Pchrome** will build for chrome only.

**-DskipTests** skip all unit tests for faster build time :)

    $cd functional-test
    $mvn verify -Dfunctional-test

You will then watch chrome/firefox pop up and the magic happen.

### Command line arguments

All the command line arguments you can change when running functional-test can be found in functional-test/pom.xml

### Running in a nested X server

**-Dwebdriver.display=$display** will run the test browser in the targeted display. This is often favourable to having a window pop up that will interrupt your work and possibly cause failures in the test.

    Xnest :1 -ac &
    icewm --display=:1 &
    mvn verify -Dfunctional-test -Dwebdriver.display=:1

### Using the test Categories
The tests are assigned to a ${Category}TestSuite, i.e. **BasicAcceptance**, **Detailed** and **Concordion** TestSuite. Running:

    mvn verify -Dfunctional-test -Dinclude.test.patterns="**/BasicAcceptanceTestSuite.java"

will execute the tests that make up the pull request validation suite.

### How to change to use chrome

Firefox (my current version 17) occasionally will hang at random. Chrome seems to run better and faster.
But unlike firefox, chrome requires a [chromedriver](http://code.google.com/p/selenium/wiki/ChromeDriver) in your system. You can download one (match your chrome version) from [here](http://code.google.com/p/chromedriver/downloads/list).

    $cd zanata-war
    $mvn clean package -Pchrome -DskipTests
    $cd ../functional-test
    $mvn verify -Dfunctional-test -Dwebdriver.type=chrome -Dwebdriver.chrome.driver=/path/to/chromedriver -Dwebdriver.chrome.bin=/path/to/google-chrome

### How to run individual test in my IDE

This assumes you have a **zanata-{version}.war** already build and you are currently under **functional-test** folder.

    $mvn integration-test -Dfunctional-test -Dcargo.wait
    $ ...
    $[INFO] Press Ctrl-C to stop the container...

Now JBoss is running and zanata is deployed. You can now run tests in IDE.