If you don't have a Fedora box, setting up a cloud one may be handy. i.e. https://pilot.dropbear.engineering.redhat.com

First you should have some basic knowledge of [how to create a RPM](http://fedoraproject.org/wiki/How_to_create_an_RPM_package) 

For advanced technique and trick, you may also want to know 
* [koji](https://fedoraproject.org/wiki/Using_the_Koji_build_system)
* [mock](http://fedoraproject.org/wiki/Using_Mock_to_test_package_builds)

Then follow [Fedora java packaging guideline](https://fedoraproject.org/wiki/Packaging:Java). This guideline is regularly updated (which means your spec will need to change accordingly). It's good idea to subscribe to java development mailing list at java-devel@lists.fedoraproject.org.

Once you have the spec file ready, you can test build it in Fedora. 

Quick Summary Note for building RPM
---
    #yum install fedora-packager rpm-build koji fedpkg rpmlint rpmdevtools

edit you ~/.rpmmacros so following lines(change to your name and email) are included:

    %home %(echo $HOME)
    %packager Patrick Huang <pahuang@redhat.com>
    %vendor Red Hat, Inc.

create you build structure under home

    $cd 
    $rpmdev-setuptree
    $cd rpmbuild

put your spec file in SPECS folder and your source tarball to SOURCES. You can use following to download source if your Source0 url is valid in your spec.

    $spectool -g SPECS/myspec.spec

build it locally

    $rpmbuild -ba SPECS/myspec.spec

or build source rpm and then let koji build it in other platform(i.e. f19, rawhide)

    $rpmbuild -bs SPECS/myspec.spec 
    $#this should output something like "/home/cloud-user/rpmbuild/SRPMS/myspec-1.0-1.fc17.src.rpm"
    $koji build --scratch f19 SRPMS/myspec-1.0-1.fc17.src.rpm

### Using mock to build chain of packages
If you have package B depends on A and they are both new packages not in Fedora yet, you can only build it locally (as oppose to build it in koji)

Install mock

    #yum install mock
    #cd /etc/mock
    #ln -s --force SOMECONFIG.cfg default.cfg #i.e. link fedora-rawhide-x86_64.cfg as default.cfg

Refer to steps in [here](http://fedoraproject.org/wiki/Using_Mock_to_test_package_builds#Building_packages_that_depend_on_packages_not_in_a_repository)

### Thins to do after your spec works
Refer to [Package review process](http://fedoraproject.org/wiki/Package_Review_Process)