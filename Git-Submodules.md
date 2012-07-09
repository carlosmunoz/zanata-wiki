# How to deal with Zanata's multiple git repos

# Introduction

Zanata has been split into several repos, so that we can release them separately, and so that you can check out the client + api without having to check out (and build) the server.

However, it is sometimes useful to check them all out and build them together.

This is where the zanata-root repo comes in.  It defines a workspace pom (ie a list of Maven `<module>`s to build together) and also has a number of git submodules, one for each of the other Zanata module repos.

## Using the Zanata submodules

You can check out the zanata-root repo like this:

    git clone git@github.com:zanata/zanata-root.git --recursive

This will check out the root pom, and also check out each of the submodules.

NB: For now, we don't plan to use submodules for tagging purposes, just as a way of tying all the modules together

## NOT using the submodules (untested)

    git clone git@github.com:zanata/zanata-root.git

This will check out the root pom, but not the submodules.  This is untested, but you might be able to check out the other modules manually, and manage them yourself.  Something like this:

    cd zanata-root
    git clone git@github.com:zanata/zanata-parent.git parent
    git clone git@github.com:zanata/zanata-api.git api
    git clone git@github.com:zanata/zanata-common.git common
    git clone git@github.com:zanata/zanata-client.git client
    git clone git@github.com:zanata/zanata.git server

Or you can ignore zanata-root entirely, and just check out the individual modules.  zanata-root is really only needed if you want to be able to build everything in a single Maven invocation.


## Update all modules to their master branches

    git submodule foreach "git checkout integration/master && git pull origin master"


# References

Using submodules
- http://chrisjean.com/2009/04/20/git-submodules-adding-using-removing-and-updating/
- http://book.git-scm.com/5_submodules.html
- http://progit.org/book/ch6-6.html
- http://longair.net/blog/2010/06/02/git-submodules-explained/


Rewriting git history (ONLY when splitting out a repo)
- http://stackoverflow.com/questions/359424/detach-subdirectory-into-separate-git-repository