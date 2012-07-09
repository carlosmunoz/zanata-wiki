# Selenium Test Case As Team Lead.

# Introduction

There are two parallel line of procedures: Local console UI & Remote Flies UI.

# Details

## Flies Procedures

1. Prepare Flies server. Write download the URL of the server. (Skip this step if you are not using local deployed server.)
1. Have user account prepared in Flies. Write down the Username and API key from profile page.
1. Have project prepared in Flies. Write down the project slug.
1. Have iteration prepared in Flies, under the project prepared in last step. Write down the iteration slug.

## Console Procedures

1. Check out from repository of Fedora Hosted:

    $ svn co http://svn.fedorahosted.org/svn/publican/trunk/publican/[ProjectName] # You could use "user-guide" here as example.

1. Download and install flies-publican client RPM. (Please check out with caius.chance for URL.)
1. Import the document into the project using the flies-publican client:

    $ flies-publican upload --user [Username] --key [APIKey] --src [LocalRepoPath] --dst http://[ServerURLAndPort]/flies/seam/resource/restv1/projects/p/[ProjectSlug]/iterations/i/[IterarionSlug]/documents

1. The documents should have been imported to Flies server.