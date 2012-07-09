# Building a web test case with selenium.

Table of contents
<wiki:toc max_depth="3" />

# Introduction

[Selenium](http://seleniumhq.org/) is a suite of tools which automate test on web applications.
In this project, [Selenium IDE](http://seleniumhq.org/projects/ide/) is used for generating and dry-run the test cases and test suites, which will then be integrated to the main testing framework.

Refer http://seleniumhq.org/docs/ for further documentation.


# Glossaries

- Test Case: A list of selenium commands (selenese) that perform a certain action.
                e.g. Login, Logout, go to Japanese translation...
- Test Suite: A list of test cases for performing a certain task.
- Test Root: The directory that contains all the tests.
                By default, it is at the subdirectory `flies-selenium/src/test/resources`.
- Privilege tests: Those tests require special care, thus are put in "privilege-test-suites".
- Normal tests: Non-privilege related tests are deemed to be "normal test".

# Prepare the test system

## Required packages

- cmake >= 2.4
- perl >=4
- Firefox. And other browsers you want to test with.
- Firefox plugins
- Selenium IDE
- firebug or DOM inspector.
- xpather, if xpath is used as main locating strategy.
- Selenium server
- For Fedora after 12, just use:
        yum install selenium-server
- Other system can follow the [installation instruction](http://seleniumhq.org/docs/05_selenium_rc.html#installation).

## Clone the source code

Get a local copy of the flies tests repository with this command:
    hg clone https://tests.flies.googlecode.com/hg/ flies-tests  

## Configure

Most test system configuration options are in `test.cfg`.
These options can also be defined in either environment variables,
or use cmake definition (-Dvar=value).

Note that cmake definitions override environment variables, which also
override the variables defined in test.cfg.

### Server profiles

A server profile defines the flies server properties, such as server base url,
path, admin user, admin key, and so on.

The syntax of define a server property is:
    <profileName>-<properties>=<value>

For example we can define a profile named `fliesTest` by putting following in `test.cfg`
    fliesTest_SERVER_BASE="https://flies-test.example.com"
    fliesTest_SERVER_PATH="/flies/"
    fliesTest_ADMIN_USER=admin
    fliesTest_ADMIN_KEY=${ADMIN_KEY}
    fliesTest_KERBEROS=0
    filesTest_TEST_ROLES="ADMIN;PRJMANT"


### =Server Properties=

<dl>
   <dt>`SERVER_BASE`</dt>
     <dd>URL of files server, without path. e.g. `http://flies.server.org`</dd>
   <dt>`SERVER_PATH`</dt>
     <dd>Path tofiles server,  e.g. `/flies/`.
   <dt>`ADMIN_USER`</dt>
     <dd>Account name of the admin user</dd>
   <dt>`ADMIN_PASSWD`</dt>
     <dd>Password of the admin user</dd>
   <dt>`ADMIN_KEY`</dt>
     <dd>API key of the  admin user</dd>
   <dt>`KERBEROS`
     <dd>0: (Default) the flies server does not use kerberos;
         1: the flies server uses kerberos.
   <dt>`TEST_ROLES`</dt>
     <dd>Roles to be used to test
         <ul>
            <li>`ADMIN`: Super user
            <li>`PRJMANT`: Project maintainer
            <li>`TRANS`: Translator
            <li>`LOGIN`: Registered user, but without any other permission

### Translation Projects

Define translation projects will be pulled down from upstream sources,
manipulate with publican if `publican.cfg` is found, then push to flies server.

The syntax of define a server property is:
    <translationProjectSlug>-<properties>=<value>
`traslationProjectSlug` is an unique name (slug) of a project. 
It will appear as a directory name under `SAMPLE_PROJ_DIR` and become slug of flies project.

Using fedora release note as an example:
    ReleaseNotes_REPO_TYPE=git
    ReleaseNotes_NAME="Fedora Release Notes"
    ReleaseNotes_DESC="Fedora Documentation - Release Notes"
    ReleaseNotes_VERS="f13;f14"
    ReleaseNotes_URL_f13="git://git.fedorahosted.org/git/docs/release-notes.git"
    ReleaseNotes_URL_f14="git://git.fedorahosted.org/git/docs/release-notes.git"

For each translation project, there following properties:
<dl>
   <dt>`REPO_TYPE`</dt>
     <dd>Type of the translation project hosting repository. Currently support `git` and `svn`.</dd>
   <dt>`NAME`</dt>
     <dd>Project name to be used as flies project name. Can contain space.
   <dt>`DESC`</dt>
     <dd>Project description to be used as flies project description.  Can contain space.
   <dt>`VERS`</dt>
     <dd>Project versions to be push to flies server. For projects hosting with git and svn, use the branch names. Multiple version can be split by `;`</dd>
   <dt>`URL_<ver>`</dt>
     <dd>URL for accessing a specified version/branch of a translation project.</dd>


After defining translation projects, list all translation projects in `PUBLICAN_PROJECTS`, like this:
    PUBLICAN_PROJECTS="AboutFedora;DeploymentGuide;DocUtil;HibernateCore;ReleaseNotes;Readme"

### Other Configuration Options

Options that are most likely to be configured:
<dl>
   <dt>`RESULT_DIR`</dt>
     <dd>The directory to put the test result and logs.</dd>
   <dt>`TEST_ROOT`</dt>
      <dd>The directory that contain normal tests</dd>
   <dt>`PRIVILEGE_TEST_ROOT`</dt>
      <dd>The directory that contain privilege tests</dd>
   <dt>`SAMPLE_PROJ_DIR`</dt>
       <dd>Where should translation projects be downloaded to and stored.</dd>
   <dt>`LANGS`</dt>
       <dd>Language locales you want to build and update po, e.g. "ja;zh_TW"</dd>
   <dt>`BROWSERS_TO_TEST`</dt>
      <dd>Lists the browser to be tested. Current support values: `firefox`, `googlechrome`.<br/>Default value is: firefox;googlechrome</dd>
   <dt>`SELENIUM_SERVER_PORT`<dt>
       <dd>port for selenium server<br />Default value: 4444</dd>
    
# Making Tests

## Normal Tests

### File naming convention

Test case and suite files should be named according to file naming convention,
therwise those test files won't be picked up by test system.
- Test Case: A .html file that start with an English alphabet.
- Test Suite: A .html file with the prefix "0-".
   The middle name will be use as test name.
   For example, name the suite "0-Tribe_JoinLeave.html",
   if the test name "Tribe_JoinLeave" is desired.

It's recommended that a test suite and its corresponding test cases occupy their own directory. This directory should be under the test root.

If the test suite is related to an issue, it's also good to include the issue number       in the test suite name.

### Edit test suites and test cases in IDE

Note that: test suites does not need to include login/logout functions, because test system will add the them automatically.

The fastest and simplest way to make test cases and suites are using the built-in recording function. 
Click on the  red dot on the upper-right corner to activate/deactivate the recording.

However, what it generates is not necessary human-readable, as it tends to use id instead of content texts.
Using content texts as referencing elements has following benefits:
- Human readable, thus easier to debug, etc.
- More robust test if the anchor texts or "keywords" do not change.
- Closer to the actual behavior of the user.

Thus, it's recommended to edit the test suites and test cases and make them more
- Concise
- Readable
- Modularize

See [Selenium Basics](http://seleniumhq.org/docs/02_selenium_basics.htm), [Selenese commands](http://seleniumhq.org/docs/04_selenese_commands.html) for detail selenese explanation.

### Generated tests

If the file name convention is strictly followed, then cmake will generate 4 more html files for each suite:
- `1-<SuiteName>.html` add sign in as admin in the begin of test suite. Good for trying out in IDE.
- `2-<SuiteName>.html`: similar with `1-<SuiteName>.html`, but also append sign out at the end of test suite.
- `3-<SuiteName>.html`: similar with `1-<SuiteName>.html`, but as a project manager instead.
- `4-<SuiteName>.html`: similar with `2-<SuiteName>.html`, but as a project manager instead.
- `5-<SuiteName>.html`: similar with `1-<SuiteName>.html`, but as a translator instead.
- `6-<SuiteName>.html`: similar with `2-<SuiteName>.html`, but as a translator instead.

### Tips

<dl>
   <dt>[AndWait](http://seleniumhq.org/docs/04_selenese_commands.html#the-andwait-commands) or not</dt>
      <dd>If the page will be refreshed in same tab after click, then use `AndWait`; otherwise, use no need to use `AndWait`. Opening a new tab does not cause refresh, thus don't use `AndWait`.
      </dd>
   <dt>Pause vs [waitFor](http://seleniumhq.org/docs/04_selenese_commands.html#the-waitfor-commands-in-ajax-applications)</dt>
       <dd>For AJAX web application, use `WaitFor` for certain element to show up. `Pause` does not always guarantee the necessary loading time. Nevertheless, `pause` can still be used to provide the necessary time for human to investigate change before next command begins.
       </dd>
</dl>

## Privilege Tests

Privilege tests are located in the directory privilege-test-suites.
These test will be either be run as normal user and prelogin ( unlogined users),
to see if they can access resources they should not be allowed to.

### Types of privilege tests

There are several types of privilege tests:
<dl>
   <dt>HTTP404</dt><dd>URL that should return HTTP status 404 when visit.</dd>
   <dt>PERMISSION</dt><dd>URL that should be access only by admin.</dd>
   <dt>TEXT_PRESENT</dt><dd>Text to be shown.</dd>
   <dt>TEXT_NOT_PRESENT</dt><dd>Text not to be shown.</dd>
   <dt>ELEM_PRESENT</dt><dd>Web element to be shown.</dd>
   <dt>ELEM_NOT_PRESENT</dt><dd>Web element not to be shown.</dd>


### File naming convention

Test file should be named with "`Issue<IssueNum>-TestName.suite`".
Use 0 as `IssueNum` if the suite does not associate with any issue.

### File format

Each .suite 2 fields, *type* and *data*, split by tab.
Field *type* is for type of privilege test, listed in previous section.
Field *data* is the data associate to type of privilege test, it should be:
- **URL exclude $FLIES_URL**, for type `HTTP404` or `PERMISSION`.
- **Text string**, for `TEXT_PRESENT` or `TEXT_NOT_PRESENT`.
- **Selenium locator**, for `ELEM_PRESENT` and `ELEM_NOT_PRESENT`.

A suite file can contains multiple lines, lines without the prefix "#"
will be translated to selenium test cases, except `HTTP404`, which 
is covered by **http404_check**.

# Run tests (Command line)

## In command line

Refer "Environment Variable" section for option.

Use following command to run all selenium test suites:
    ctest -V -S steer.cmake

And for `http404_check`:
    ./http404_check.sh

# Integrate with hudson

For hudson installation and configuration, see [here](http://wiki.hudson-ci.org/display/HUDSON/Use+Hudson).

Hudson will not accept graphical output, so we need to set the headless environment for hudson invoked tests. This can be done by output to a framebuffer X server.

## Install a Framebuffer X server

Install a framebuffer X server for Fedora:
       yum install xorg-x11-server-Xvfb

## Install Hudson Seleniumhq Plugin

See [Seleniumhq Plugin](http://wiki.hudson-ci.org/display/HUDSON/Seleniumhq+Plugin) for instruction.

## Add a flies-selenium project in Hudson

1. Click on "New job"
1. Type the job name as "flies-selenium` and select "Build a free-style software project", click "Ok"
1. In the next page, `Build` section, click "Add build step", select "Execute shell". Enter
    mkdir -p selenium/results
    
    # If you want log the jboss log during execution, uncomment following line
    # tail --follow=name <jboss.log> > selenium/results/jboss.log &
    
    # Run Frame buffer X on DISPLAY 98
    /usr/bin/Xvfb :98 -ac  -screen 0 1024x768x24&
    export DISPLAY=:98
    
    # Uncomment these if you don't want defaults.
    # export BROWSERS_TO_TEST=firefox;googlechrome
    # export SELENIUM_SERVER_PORT=4444
    
    # Suppress bug-buddy warning 
    export GNOME_DISABLE_CRASH_DIALOG=1 
    
    cd selenium/src
    
    ctest -VV -S steer.cmake
    
    pkill -9 Xvfb
    pkill -f 'tail --follow=name <jboss.log>'

1. In post-build Actions:
1. Check *Archive the artifacts*, and fill *Files to archive* with `selenium/results/**.html`
1. Check *Publish Selenium Report*, and fill *Test report HTMLs* with `selenium/results/**.html`
1. Click `Save`

## Add a flies-http404 project in Hudson

1. Back to hudson server home page.
1. Click on "New job"
1. Type the job name as "flies-http404` and select "Build a free-style software project", click "Ok"
1. In the next page, `Build` section, click "Add build step", select "Execute shell". Enter
    mkdir -p selenium/results
    cd selenium/src
    ./http404_check.sh

1. In post-build Actions:
1. Check *Archive the artifacts*, and fill *Files to archive* with `selenium/results/**.xml`
1. Check *Publish JUnit test result report*, and fill *Test report XMLs* with `selenium/results/**.xml`

1. Click `Save`

## Run Tests

You may either click "Build Now", or set the triggers for projects for scheduled builds.

The results are visible in Work Space, under directory `selenium/results/`.
- Files with `**.html` are selenium test results,
- Files with `**.txt` are http 404 tests.

# Examples

Go to the [hg flies test repo](http://code.google.com/p/flies/source/browse/?repo=tests#hg/selenium/src) to see the up-to-date examples.