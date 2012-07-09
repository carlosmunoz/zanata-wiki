# A quick overview of Getting Started.

# Getting Started

This chapter introduces you to the basic functions required before a user can start translating documents. Topics includes:

- Creating an account
- Signing In and Signing Out
- Joining a Language Team

# Translation Workflow

1. Author modifies documentation, checks in !DocBook XML source.
1. At some point in the lifecycle, a documentation freeze is announced.
1. Import job is run (eg from Hudson/Jenkins).  (See script 2 below: `zanata_import_source`.)
1. Translators can begin translating at < https://translate.jboss.org/ >.
1. Draft builds are run nightly or more often (Jenkins?).  (See script 3 below: `zanata_draft_build`)
1. If author changes any XML, go back to step 3.
1. Translations declared "final"
1. Documentation release build is run.  (See script 4 below: `zanata_export_translations`)