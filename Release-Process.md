# Release process for Zanata

# Branch management after a release

Note: This applies to api, client and server.  The other modules don't have release or legacy branches.

Note: make sure any fixes to legacy/release have been merged to the later branches, because `git reset --hard` will throw them away.

# Merging master to release and release to legacy

    git fetch
    git checkout legacy
    git merge origin/release --ff-only --quiet
    git checkout release
    git merge origin/master --ff-only --quiet
    git checkout integration/master
    mvn release:update-versions -DautoVersionSubmodules=true -DdevelopmentVersion=${developmentVersion}
    git commit pom.xml */pom.xml -m "prepare for next development iteration"
    # push all the changes back to the server
    git push origin legacy release integration/master


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

See [how to create GPG keys](http://fedoraproject.org/wiki/Creating_GPG_Keys).

You can test your GPG agent with this:

    gpg --clearsign < /dev/null
    gpg --clearsign < /dev/null

If you tell the GPG agent to remember your passphrase, the second invocation of GPG should not ask for it again.

You will also need an OSSRH login which has been enabled for the groupId org.zanata.   https://docs.sonatype.org/display/Repository/Sonatype+OSS+Maven+Repository+Usage+Guide

Create an account at https://issues.sonatype.org/ and then sflaniga can request to have you added to the org.zanata group.

https://issues.sonatype.org/browse/OSSRH-3981
https://issues.sonatype.org/browse/OSSRH-3982
https://issues.sonatype.org/browse/OSSRH-3983


You will need to add your sonatype credentials to `~/.m2/settings.xml`.  First you should create a user token, to avoid putting your actual username and password in the file.  Visit https://oss.sonatype.org/index.html#profile and select User Token from the dropdown list, then Access User Token.  Once you have that, put it into `~/.m2/settings.xml`:

	<servers>
		<server>
			<id>sonatype-nexus-snapshots</id>
			<!-- token for myRealUsername	-->
			<username>myTokenUsername</username>
			<password>myTokenPassword</password>
		</server>
		<server>
			<id>sonatype-nexus-staging</id>
			<!-- token for myRealUsername	-->
			<username>myTokenUsername</username>
			<password>myTokenPassword</password>
		</server>
	</servers>

Then you should be able to make the release:

    # change maven.repo.local as appropriate, but best not to use your normal work repo 
    git pull
    mvn -Dmaven.repo.local=$HOME/NotBackedUp/maven-central-release-repo \
      release:clean release:prepare release:perform &&
    # close and release on OSSRH

    # obsolete: mvn nexus:staging-close nexus:staging-release \
    #  -Dnexus.automaticDiscovery -Dnexus.groupId=org.zanata \
    #  -DtargetRepositoryId=releases -Dnexus.promote.autoSelectOverride
    # untested alternative:
    mvn nexus-staging:release -Psonatype-oss-release \
        -DaltStagingDirectory=target/checkout/target/nexus-staging

## Releasing Tennera (JGettext) ##
    cd tennera; git pull
    mvn -Dmaven.repo.local=$HOME/NotBackedUp/maven-central-release-repo \
      -Dgpg.useagent \
      release:clean release:prepare release:perform &&
    # obsolete: mvn nexus:staging-close nexus:staging-release \
    #  -Dnexus.automaticDiscovery -Dnexus.groupId=org.fedorahosted.tennera \
    #  -DtargetRepositoryId=releases -Dnexus.promote.autoSelectOverride
    # untested alternative:
    mvn nexus-staging:close nexus-staging:release

## Releasing OpenProps ##
    cd openprops; git pull
    mvn -Dmaven.repo.local=$HOME/NotBackedUp/maven-central-release-repo \
      -Dgpg.useagent \
      release:clean release:prepare release:perform &&
    # obsolete: mvn nexus:staging-close nexus:staging-release \
    #  -Dnexus.automaticDiscovery -Dnexus.groupId=org.fedorahosted.openprops \
    #  -DtargetRepositoryId=releases -Dnexus.promote.autoSelectOverride
    # untested alternative:
    mvn nexus-staging:close nexus-staging:release