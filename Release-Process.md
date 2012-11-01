# Release process for Zanata

# Branch management after a release

Note: This applies to api, client and server.  The other modules don't have release or legacy branches.

# Moving master to release and release to legacy
    git fetch
    git checkout legacy && git merge origin/release
    git checkout release && git merge origin/master


# Release process for modules (except zanata-server)

Make sure you have the latest version (head) from the repository, and that you are **in the correct branch**.  

    $ git pull
    $ git co master (git co release OR legacy if releasing from a branch)

## Interactive release (for parent, api, client, common, not server)

NB: Make sure you use the correct version numbers and tags!

    $ mvn release:prepare -Dresume=false
    What is the release version for "Zanata Root POM"? (org.zanata:root) 1.1: : 
    What is SCM release tag or label for "Zanata Root POM"? (org.zanata:root) root-1.1: : 
    What is the new development version for "Zanata Root POM"? (org.zanata:root) 1.2-SNAPSHOT: : 

