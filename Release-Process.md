# Release process for Zanata

# Release and deploy modules to Maven Central (not zanata-server) ##

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

Make sure you have the latest version (head) from the repository, and that you are **in the correct branch**.  

    git checkout integration/master # (git checkout release OR legacy if releasing from a branch)

    # change maven.repo.local as appropriate, but best not to use your normal work repo 
    git pull
    mvn -Dmaven.repo.local=$HOME/NotBackedUp/maven-central-release-repo \
      release:clean release:prepare release:perform &&
    # close and release on OSSRH
    mvn nexus-staging:release -Psonatype-oss-release \
        -DstagingRepositoryId=$(grep -oP '^stagingRepository\.id=\K.*' target/checkout/target/nexus-staging/staging/*.properties)

Note that the release process might work, but nexus-staging:release fail.  If you get an error, check to see if that is what happened.  If so, you will need to release the repo manually in Nexus here: https://oss.sonatype.org/index.html#stagingRepositories

## Releasing Tennera (JGettext) ##
    cd tennera; git pull
    mvn -Dmaven.repo.local=$HOME/NotBackedUp/maven-central-release-repo \
      -Dgpg.useagent \
      release:clean release:prepare release:perform &&
    mvn nexus-staging:close nexus-staging:release

## Releasing OpenProps ##
    cd openprops; git pull
    mvn -Dmaven.repo.local=$HOME/NotBackedUp/maven-central-release-repo \
      -Dgpg.useagent \
      release:clean release:prepare release:perform &&
    mvn nexus-staging:close nexus-staging:release

# Branch management after a release

Note: This applies to api, common, client and server. Parent doesn't have release or legacy branches.

Note: make sure any fixes to legacy/release have been merged to the later branches, because `git reset --hard` will throw them away.

# Merging master to release (for API, common, Client). Only needed when you want to start a new x.y release.
**NB: Make sure you fill in ${developmentVersion}, eg 3.1-SNAPSHOT**

    git fetch
    git checkout release
    git merge origin/release --ff-only --quiet
    git merge origin/master --ff-only --quiet
    git checkout integration/master
    git merge origin/integration/master --ff-only --quiet
    mvn release:update-versions -DautoVersionSubmodules=true -DdevelopmentVersion=${developmentVersion}
    git commit pom.xml */pom.xml -m "prepare for next development iteration"
    # push all the changes back to the server
    git push origin release integration/master

# Merging release to legacy and master to release (for Server)
**NB: If you want to automate this, make sure you fill in ${developmentVersion}, eg 3.1-SNAPSHOT**

If Zanata's version of JBoss EAP has been updated recently, you may need to update the configuration management manifests, to ensure that the correct version of EAP is deployed to each test machine.  In some cases you may also need to change .config/zanata-deploy.conf on the build machine, to ensure that zanata.war is still deployed correctly on each test machine and that the jbossas service is managed correctly.

    git fetch
    git checkout integration/master && git reset --hard origin/integration/master
    git branch -D legacy release || true

    git checkout legacy
    git merge origin/release --ff-only --quiet ||
    (echo please check for cherry-picked commits in legacy which were never merged into release; exit 1)

    git checkout release
    git merge origin/master --ff-only --quiet ||
    (echo please check for cherry-picked commits in release which were never merged into master; exit 1)

    git checkout integration/master
    mvn release:update-versions -DautoVersionSubmodules=true -DdevelopmentVersion=${developmentVersion}
    git commit pom.xml */pom.xml -m "prepare for next development iteration"

    # push all the changes back to the server
    git push origin legacy release integration/master