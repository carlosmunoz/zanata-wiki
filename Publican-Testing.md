# Collection of publican projects and Test Results

# Fedora Related Documents

Projects gathered from [this Transifex search result](https://translate.fedoraproject.org/search/?q=docs).

<table>
  <tr><td>Flag</td><td>Name</td><td>Repo</td><td>Rev</td><td>Size</td><td>Result</td></tr>
  <tr><td>✔</td><td>Docs :: About Fedora</td><td>`git://git.fedorahosted.org/git/docs/about-fedora.git`</td><td>14fac71583206de78b178dba60e253c172e1c4a0</td><td>4.2MB</td><td>No problem.</td></tr>
  <tr><td>✔</td><td>Docs :: Accessibility Guide</td><td>`git://git.fedorahosted.org/git/accessibility-guide.git`</td><td>91c944e8320492ab9e0846155324b3902c8f27f8</td><td>2.7MB</td><td>No problem.</td></tr>
  <tr><td>X</td><td>Docs :: Common entities</td><td>`git://git.fedorahosted.org/git/fedora-doc-utils.git`</td><td>c233dc87304a42f44a1d394c4e284d69cebf1380</td><td>8.9MB</td><td>Not publican format.</td></tr>
  <tr><td>✔</td><td>Docs :: Deployment Guide</td><td>`git://git.fedorahosted.org/git/docs/deployment-guide.git`</td><td>4432a67758550fba209c27960335a7b6fbc44f8f</td><td>276MB</td><td>Issue#24</td></tr>
  <tr><td>✗</td><td>Docs :: Documentation Guide</td><td>`pserver:anonymous@cvs.fedoraproject.org:/cvs/docs/documentation-guide`</td><td>revision 1.21</td><td>7.3MB</td><td>Old publican format. `publican old2new` has errors that find-makefile-common command is not available.</td></tr>
  <tr><td>✗</td><td>Docs :: Homepage</td><td>`git://git.fedorahosted.org/git/docs/homepage.git`</td><td>683b059ff25d281bbe55249a62f06234f726aa22</td><td>2.0M</td><td>Not publican format.</td></tr>
  <tr><td></td><td>Docs :: Installation Guide</td><td>`git://git.fedorahosted.org/git/docs/install-guide.git`</td><td>ed514ae290949c8f9484dc90d1a51435efc62bce</td><td>566MB</td><td></td></tr>
  <tr><td>✔</td><td>Docs :: Installation Quick Start GUide</td><td>`git://git.fedorahosted.org/git/installation-quick-start-guide.git`</td><td>2085f825bdf285253d595c4b6f2d7a3a2e667326</td><td>59MB</td><td>No problem.</td></tr>
  <tr><td>✔</td><td>Docs :: Docs :: Jargon Buster</td><td>`git://git.fedorahosted.org/git/docs/readme.git`</td><td>ad2eadb81f20f09b58fdaa4e8363ff209c91c085</td><td>612KB</td><td>No problem.</td></tr>
  <tr><td>✔</td><td>Docs :: Readme</td><td>`git://git.fedorahosted.org/git/docs/readme.git`</td><td>1057c0e2edc88a336edf8cbaa8257026e0a6951b</td><td>5.0MB</td><td>No problem.</td></tr>
  <tr><td>✗</td><td>Docs :: Readme Burning ISOs</td><td>`git://git.fedorahosted.org/git/docs/readme-burning-isos.git`</td><td>d1a866f5708545a3a694c5c04b5504326c6296d6</td><td>6.6MB</td><td>Issue#18</td></tr>
  <tr><td>✔</td><td>Docs :: Readme Live Image</td><td>`git://git.fedorahosted.org/git/docs/readme-live-image.git`</td><td>d627863de1ec93b3e5411d8d1d82f60099fde3ec</td><td>4.3MB</td><td>No problem.</td></tr>
  <tr><td>✔</td><td>Docs :: Release Notes</td><td>`git://git.fedorahosted.org/git/docs/release-notes.git`</td><td>640d8a4f8679270e1e1f9acccb61a5fb6d926a03</td><td>57MB</td><td>No problem.</td></tr>
  <tr><td>✗</td><td>Docs :: Security Guide</td><td>`http://svn.fedorahosted.org/svn/securityguide/community/f13`</td><td>revision 284</td><td>21MB</td><td>Issue#32</td></tr>
  <tr><td>✗</td><td>Docs :: SELinux User Guide</td><td>`http://svn.fedorahosted.org/svn/selinuxguide/community/trunk/SELinux_User_Guide/`</td><td>revision 401</td><td>22MB</td><td>Issue#31</td></tr>
  <tr><td>✔</td><td>Docs :: Translation Quick Start Guide</td><td>`git://git.fedorahosted.org/git/translation-quick-start-guide.git`</td><td>757c8900e46bd3d2fe3d4a9741f633046a67245c</td><td>6.9MB</td><td>Issue#18</td></tr>
  <tr><td>✔</td><td>Docs :: User Guide</td><td>`git://git.fedorahosted.org/git/docs/user-guide.git`</td><td>11e11b90cfc468c774650426216a2ee6d5e8e7a0</td><td>28MB</td><td>No problem.</td></tr>
  <tr><td>✔</td><td>Docs :: Virtualization Guide</td><td>`git://git.fedorahosted.org/git/Virtualization_Guide.git`</td><td>4f736b3af487cf009b518b45e9657b1e0804a15f</td><td>50MB</td><td>No problem.</td></tr>
</table>

# JBoss.org Related Documents

<table>
  <tr><td>Flag</td><td>Name</td><td>Repo</td><td>Rev</td><td>Size</td><td>Result</td></tr>
  <tr><td>✔</td><td>Hibernate Core</td><td>`http://anonsvn.jboss.org/repos/hibernate/core/trunk/documentation/manual/src/main/docbook/`</td><td>revision 19542</td><td>25MB</td><td>No problem.</td></tr>
  <tr><td>✔</td><td>Seam Reference Guide</td><td>`http://anonsvn.jboss.org/repos/seam/docs/trunk/Seam_Reference_Guide/`</td><td>revision 11325</td><td>103MB</td><td>Take very long (~30 mins) but finally got there.</td></tr>
  <tr><td>✔✗</td><td></td><td></td><td></td><td></td><td></td></tr>
</table>

# Other
