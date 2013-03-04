# Release process for Zanata

# Branch management after a release

Note: This applies to api, client and server.  The other modules don't have release or legacy branches.

# Merging master to release and release to legacy
    git fetch
    git checkout legacy && git merge origin/release
    git checkout release && git merge origin/master
    update versions in master's pom files (possibly use the maven versions plugin: mvn release:update-versions -DautoVersionSubmodules=true)

# Release process for modules (except zanata-server)

Make sure you have the latest version (head) from the repository, and that you are **in the correct branch**.  

    $ git checkout integration/master # (git checkout release OR legacy if releasing from a branch)
    $ git pull

## Interactive release (for parent, api, client, common, not server)

NB: Make sure you use the correct version numbers and tags!

    $ mvn release:prepare -Dresume=false
    What is the release version for "Zanata Root POM"? (org.zanata:root) 1.1: : 
    What is SCM release tag or label for "Zanata Root POM"? (org.zanata:root) root-1.1: : 
    What is the new development version for "Zanata Root POM"? (org.zanata:root) 1.2-SNAPSHOT: : 


## Release and deploy to Maven Central ##

You will need a GPG key, and you will need the GPG agent running.  You may want to tell the GPG agent to sign without asking temporarily, or else it will ask you over and over.  

You will also need an OSSRH login which has been enabled for the groupId org.zanata.   https://docs.sonatype.org/display/Repository/Sonatype+OSS+Maven+Repository+Usage+Guide

Create an account at https://issues.sonatype.org/ and then sflaniga can request to have you added to the org.zanata group.

https://issues.sonatype.org/browse/OSSRH-3981
https://issues.sonatype.org/browse/OSSRH-3982
https://issues.sonatype.org/browse/OSSRH-3983


    # change maven.repo.local as appropriate, but best not to use your normal work repo 
    git pull
    mvn -Dmaven.repo.local=$HOME/NotBackedUp/maven-central-release-repo \
      release:clean release:prepare release:perform &&
    # close and release on OSSRH
    mvn nexus:staging-close nexus:staging-release \
      -Dnexus.automaticDiscovery -Dnexus.groupId=org.zanata \
      -DtargetRepositoryId=releases -Dnexus.promote.autoSelectOverride

## Releasing Tennera (JGettext) ##
    cd tennera; git pull
    mvn -Dmaven.repo.local=$HOME/NotBackedUp/maven-central-release-repo \
      release:clean release:prepare release:perform &&
    mvn nexus:staging-close nexus:staging-release \
      -Dnexus.automaticDiscovery -Dnexus.groupId=org.fedorahosted.tennera \
      -DtargetRepositoryId=releases -Dnexus.promote.autoSelectOverride

## Releasing OpenProps ##
    cd openprops; git pull
    mvn -Dmaven.repo.local=$HOME/NotBackedUp/maven-central-release-repo \
      release:clean release:prepare release:perform &&
    mvn nexus:staging-close nexus:staging-release \
      -Dnexus.automaticDiscovery -Dnexus.groupId=org.fedorahosted.openprops \
      -DtargetRepositoryId=releases -Dnexus.promote.autoSelectOverride
