# Demonstration of working on publican standardized document sources with Flies.

# Introduction

This page demonstrates how localization teams works on publican standardized document sources with Flies. The directory locations and other parameters are specified to author's development environment. Please customize it if needed.

For how to test on local JBoss server, please check out [How to Test on Local JBoss](http://code.google.com/p/flies/wiki/HowToTestOnLocalJboss).

For how to build a document, please check out [Publican User Guide](http://jfearn.fedorapeople.org/Publican/chap-Users_Guide-Creating_a_document.html#sect-Users_Guide-Building_a_document).

# Preparation

1. Make sure a translator has its Flies account created and joined corresponding "Tribes" *(ja-JP / Japanese-Japan in this case)*.
1. Download and install flies-publican client package:
    $ rpm -Uvh flies-publican-[ver-rel].rpm
1. Check out publican standardized document sources from repository:
    $ svn co http://svn.fedorahosted.org/svn/publican/trunk/publican/Users_Guide
1. Create POT files from Publican XML files:
    $ publican update_pot
1. Update any existing PO files from Publican XML files:
    $ publican update_po --langs=ja-JP
1. Import PO/POT files to Flies *(Add --importpo to import existing translations from PO files. CAUTION: this may overwrite/delete translations stored in Flies!  Don't use --importpo if Flies translation work has started)*:
    $ flies-publican upload --user admin --key 12345678901234567890123456789012 --src ./ --dst http://localhost:8080/flies/seam/resource/restv1/projects/p/sample-project/iterations/i/1.1/documents
1. Translators open a document in Flies and translate.
1. Export PO/POT files from Flies *(Add --exportpot if POT files are needed)*:
    $ flies-publican download --user admin --key 12345678901234567890123456789012 --src http://localhost:8080/flies/seam/resource/restv1/projects/p/sample-project/iterations/i/1.1/documents --dst ./

Note that you will probably need to change the API key and flies URLs given above!