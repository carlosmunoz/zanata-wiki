If you don't have a Fedora box, setting up a cloud-based instance may be handy.

First you should have some basic knowledge of [how to create an RPM](http://fedoraproject.org/wiki/How_to_create_an_RPM_package).

For advanced technique and trick, you may also want to know [Koji](https://fedoraproject.org/wiki/Using_the_Koji_build_system) and/or [mock](http://fedoraproject.org/wiki/Using_Mock_to_test_package_builds)

Then follow [Fedora Java packaging guideline](https://fedoraproject.org/wiki/Packaging:Java). This guideline is updated regularly (which means your spec will need to change accordingly). It's a good idea to subscribe to the Java development mailing list at java-devel@lists.fedoraproject.org.

Once you have the spec file ready, you can test build it in Fedora. 

Quick Summary Note for building RPM
---
    #yum install fedora-packager rpm-build koji fedpkg rpmlint rpmdevtools

edit your ~/.rpmmacros so following lines (change accordingly) are included:

    %home %(echo $HOME)
    %packager First Last <name@yourmail.com>
    %vendor Red Hat, Inc.

create your build structure under $HOME

    $cd 
    $rpmdev-setuptree
    $cd rpmbuild

put your spec file in SPECS folder and your source tarball to SOURCES. You can use the following to download source if your Source0 url is valid in your spec.

    $spectool -g SPECS/myspec.spec

build it locally

    $rpmbuild -ba SPECS/myspec.spec

or build source rpm and then let Koji build it in other platform(i.e. f19, rawhide)

    $rpmbuild -bs SPECS/myspec.spec 
    $#this should output something like "Wrote /home/cloud-user/rpmbuild/SRPMS/myspec-1.0-1.fc17.src.rpm"
    $koji build --scratch f19 SRPMS/myspec-1.0-1.fc17.src.rpm

### Using mock to build chain of packages
If you have package B which depends on A and they are both new packages (not in Fedora yet), you can only build it locally (as oppose to build it in Koji)

Install mock

    #yum install mock
    #cd /etc/mock
    #ln -s --force SOMECONFIG.cfg default.cfg #i.e. link fedora-rawhide-x86_64.cfg as default.cfg

Refer to steps [here](http://fedoraproject.org/wiki/Using_Mock_to_test_package_builds#Building_packages_that_depend_on_packages_not_in_a_repository)

### Thins to do after your spec works
Refer to [Package review process](http://fedoraproject.org/wiki/Package_Review_Process)

Summary note for new package review
---
1. [Request for review in bugzilla](https://bugzilla.redhat.com/bugzilla/enter_bug.cgi?product=Fedora&format=fedora-review)
2. Find a reviewer. Any package maintainer will do. In our team, dchen, sflanigan and pahuang. You can also try your luck by sending an email to java-devel mailing list or block [Java SIG tracker bug](https://bugzilla.redhat.com/show_bug.cgi?id=652183)
    * Reviewer should assign the bug to himself and set flag fedora-review to ?
    * Reviewer can use a tool called [fedora-review](https://github.com/timlau/FedoraReview) to automate the review process. (#yum install fedora-review)
    * Once reviewer approves it, he/she sets flag fedora-review to + and puts review result in bug comment.
3. [Request for SCM creation ](http://fedoraproject.org/wiki/Package_SCM_admin_requests) 
    * Set flag fedora-cvs to ?
    * Add comment

            New Package SCM Request
            =======================
            Package Name: 
            Short Description: 
            Owners: 
            Branches: 
            InitialCC: 

4. Once git repo is created, [import your package into SCM](http://fedoraproject.org/wiki/Using_git_FAQ_for_package_maintainers#How_do_I_import_a_SRPM_package.3F)

        $fedora-packager-setup # you only need to run this once to enable your access to fedora git repo
        $fedpkg clone <package>
        $git checkout master # should already in master
        $fedpkg import ~/rpmbuild/SRPM/myspec-1.0-1.fc17.src.rpm
        $git commit
        $git push
        $fedpkg build
        #once build successful, repeat the process for other branch, i.e. f19, f18, f17
        $fedpkg switch-branch f19
        $git merge master
        $git push
        $fedpkg build
5. Create [Bodhi](http://fedoraproject.org/wiki/Bodhi) request to push your package into Fedora
    * Go to [Bodhi](https://admin.fedoraproject.org/updates).
    * Create New Update -> enter package name (name-version-release.platform, i.e. zanata-api-2.2.0-6.fc18. Or you can type and let the ajax load it), choose new package, choose testing, then copy paste description as note and input review bug number.
    * Wait for it to pass testing (generally 7 days).
    * Once testing request is ok in Bodhi, open the request and click *Mark as Stable*.

#### Hint
* You can not submit review request if the package being built depends on another package which is under review.
* If package A depend on B, and B is still in testing, you can click on Buildroot Overrides in Bodhi and submit a request. Fill in B as package that is in testing, a note which says why you want to override, expiry day can be set to B's expected go stable date or a bit later, then submit. You can then build in Koji for A.