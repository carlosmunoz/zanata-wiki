# Developing Zanata with GWT DevMode

# Introduction

Instead of re-compiling and re-deploying zanata.war after every change, you can test Zanata UI changes much more easily by using GWT DevMode in Eclipse.

## webtrans-dummy

From Eclipse's Run/Debug menu, choose the Web Application profile named `webtrans-dummy`.  You don't need to have JBoss running.

This mode uses DevMode's built-in web server with a dummy Zanata server.  All GWT-RPC calls will return dummy data (or no data, if a dummy module hasn't been written yet).  See the class `DummyDispatchAsync` for the list of dummy modules, or to add one of your own.  The Dummy modules live in the package `org.zanata.webtrans.client.rpc`, but under `src/test/java` instead of `src/main/java`.

## webtrans-jboss

From Eclipse's Run/Debug menu, choose the Web Application profile named `webtrans-jboss`.  Make sure you have JBoss running, and zanata.war successfully deployed.

You will need to create a String Substitution variable in Eclipse's Preferences, named `deploy.zanata`, with the full pathname of your jboss deploy directory, eg `/home/user/apps/jboss-ewp-5.0/jboss-as-web/server/default/deploy/zanata.war`