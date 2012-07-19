# Release process for Zanata

# Creating a stabilisation branch (before a release)

This branch will only accept bug-fixes leading up to a release.  After the release, it becomes the maintenance branch for that release.

TODO: try http://maven.apache.org/plugins/maven-release-plugin/examples/branch.html to automate this

After the branch for Zanata x.y is created, that branch should be the only place where Zanata x.y gets built in future, so we immediately update the poms in the master branch to x.(y+1)-SNAPSHOT.

    $ # make sure your working directory is clean!
    $ git co integration/master
    $ git pull
    $ git branch x.y # fill in x.y as appropriate!
    # still in master branch:
    $ mvn release:update-versions (-B to accept defaults)
    # specify x.(y+1)-SNAPSHOT when asked for the new version
    $ git add .
    $ git ci -m "update pom versions for next development iteration"
    $ git push
    $ git push origin integration/master x.y

NB: Build jobs may need to be updated to use the new branch.

# Creating a maintenance branch (after a release)
NB: don't do this if you created a stabilisation branch before the release.  Just start using the stabilisation branch as a maintenance branch.

Check out the commit which came just before `[maven-release-plugin]`'s first commit, eg

    git checkout <commit>
    git checkout -b <branch-name>
    git push origin <branch-name>

NB: Build jobs may need to be updated to use the new branch.

# Release process (OLD information - use Jenkins for automated releases)

Make sure you have the latest version (head) from the repository, and that you are **in the correct branch**.  

    $ git pull
    $ git co master (git co x.y if releasing from a branch)

## Interactive release

NB: Make sure you use the correct version numbers and tags!

Test run:

    $ mvn release:prepare -Dresume=false -DdryRun=true -Darguments='-DskipTests -Dgwt.compiler.skip'
    What is the release version for "Zanata Root POM"? (org.zanata:root) 1.1: : 1.2-alpha-2
    What is SCM release tag or label for "Zanata Root POM"? (org.zanata:root) root-1.1: : zanata-1.2-alpha-2
    What is the new development version for "Zanata Root POM"? (org.zanata:root) 1.2-SNAPSHOT: : 1.2-SNAPSHOT

Hit ^C if you get sick of waiting...

The real thing:

    $ mvn release:prepare -Dresume=false -Darguments='-Dgwt.validateOnly'
    What is the release version for "Zanata Root POM"? (org.zanata:root) 1.1: : 1.2-alpha-2
    What is SCM release tag or label for "Zanata Root POM"? (org.zanata:root) root-1.1: : zanata-1.2-alpha-2
    What is the new development version for "Zanata Root POM"? (org.zanata:root) 1.2-SNAPSHOT: : 1.2-SNAPSHOT

Wait while the build is performed.  

    git push; git push --tags


Now go to Jenkins and tell it to build the tag you just created.  Make sure the Maven deployment is successful; tell it to re-deploy if it fails.

## Non-interactive release

(Remember to fill in version x.y)

Prerelease:

 * $ git co **x.y** (branch name)

 * $ mvn release:prepare -Dresume=false -Darguments='-Dgwt.validateOnly' -DreleaseVersion=**x.y-alpha-z** -Dtag=**zanata-x.y-alpha-z** -DdevelopmentVersion=**x.y-SNAPSHOT**

 * $ git push; git push --tags

GA release:

 * $ git co **x.y**

 * $ mvn release:prepare -Dresume=false -Darguments='-Dgwt.validateOnly' -DreleaseVersion=**x.y** -Dtag=**zanata-x.y** -DdevelopmentVersion=**x.y.1-SNAPSHOT**

 * $ git push; git push --tags

Point/maintenance release:

 * $ hg co **x.y**

 * $ mvn release:prepare -Dresume=false -Darguments='-Dgwt.validateOnly' -DreleaseVersion=**x.y.z** -Dtag=**zanata-x.y.z** -DdevelopmentVersion=**x.y.(z+1)-SNAPSHOT**

 * $ git push; git push --tags


Now go to Jenkins and tell it to build the tag you just created.  Make sure the Maven deployment is successful; tell it to re-deploy if it fails.