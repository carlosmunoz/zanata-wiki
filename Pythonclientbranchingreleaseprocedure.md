# Introduction

The branching/release procedure of python client is intended to ensure  that the version number of python client doesn't increase too often. It also makes sure that testing is synchronised to the development of python client, so we don't need repeat testing and we can make a stable release of python client. 

# Details

1. The main branch always contains clean, stable and releasable code. The changes (bugs fix, features implementation) will be committed to devel branch. These two branch have different version numbers. For example: the main branch is 0.8.1 and devel branch is 0.8.2 (currently, the version number occurs in fliesclient/flies.py and setup.py)
1. After fixing a bug, the developer submits changes and links the issue number in the commit comment; the developer also needs to comment on the bug with commit number.
1. The version (devel or release) is read from 'VERSION-FILE', which is created by the shell script 'VERSION-GEN' (based on git-describe) ([explanation](http://stackoverflow.com/questions/514188/how-would-you-include-the-current-commit-id-in-a-git-projects-files/514668#514668)).
1. Test the commit on devel branch to verify that the commit fixes the bug or implements the feature.
1. After the commit is verified and it is decided to release a new version, the main branch will merge from the devel branch.
1. Tag the source code in main branch with new version number and push it to main git repository. Make sure it is a annotated tag and using v in tag names.
1. Update the version/release number in the fedora spec file, which is only used by fedora package build and not included in source code repository.
1. Upload and build it on koji and commit to bodhi, then test on different platform and add/remove karma in bodhi.
1. If the python client is marked stable, it will be released to the fedora `updates` repository by bodhi