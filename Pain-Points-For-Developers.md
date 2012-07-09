# Things that make Flies development painful, and possible solutions.

# Testing

- Seam-based integration tests are fairly slow (~ 20sec startup) and often don't work in IDE
- OPTION: ensure we only use Seam-based tests where appropriate
- OPTION: look at replacing seam with a more light-weight alternative (weld or guice  on tomcat, arquillian for testing)
- Exploded deployment causes webapp redeploy every time (slow)
- Changes to XHTML aren't reflected unless you redeploy
- OPTION: look at maven-war-plugin:inplace.  See http://www.vijedi.net/2010/jboss-hot-deploy-with-maven/
- Deployment wipes the Flies database every time (bad for ad hoc testing since you lose your test data)
- OPTION: modify dev profile so that it doesn't do that
- OPTION: look at restructuring our test-data around the same serialization formats that the REST api exposes

# Maven profiles

- Multiple profiles lead to multiple versions of each artifact, even when source is the same
- developers are testing one version of the code
- but we deploy another version
- Generated Eclipse config is wrong if the wrong profile is used (breaks tests)
- use alternative means of configuring Eclipse's classpath (eg directly configure maven-eclipse-plugin)
- ~~OPTION: eliminate all use of profiles somehow~~
- OPTION: make sure different profiles never affect the generated artifacts
- OPTION: maven isn't designed for this. would gradle/buildr/ant that consume/produce maven artifacts give us a simpler build?
Maven isn't designed for what, exactly?  Producing variant artifacts?  It is, but only if you use ~~qualifiers~~ [classifiers](http://www.sonatype.com/books/mvnref-book/reference/profiles-sect-platform-classifier.html) to give them different co-ordinates.  Which makes sense.  
We generate more variants than we should need.  There should be only one version of flies.war (for a given combination of source-rev/app-server/database).  We can achieve that by externalising ~~all~~ configuration properly, not by switching build systems.  -SF

[This post](http://blog.jayway.com/2010/01/21/one-artifact-with-multiple-configurations-in-maven/) looks like a reasonable way of managing multiple configurations.  (The first comment recommends loading configuration from the classpath instead, but I'm not sure how that would be set up.) -SF

# Build

- Local build is slow (modify-compile-test feedback cycle is too long)
- OPTION: replace maven with something faster
- Hudson build is slow (feedback cycle is even longer)
- OPTION: modularize build better.
- We don't have a local cache of our dependencies
- OPTION: Look at local deployment of repo manager (Nexus)?

# Eclipse

- Maven's eclipse plugin (eclipse:eclipse) isn't integrated with Eclipse - bad workflow
- OPTION: Investigate m2eclipse again?
- OPTION: replace maven?