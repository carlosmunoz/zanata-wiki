The Translation Editor (Webtrans) is a GWT application that uses . It is distributed from a Zanata server and runs in a browser (Chrome and Firefox are targeted specifically), and uses GWT-RPC to communicate with the server.

The GWT SDK is required to develop this component.

![Webtrans general architecture](http://zanata.org/images/diagrams/zanata-2.0-architecture-webtrans.svg)

As well as core GWT libraries, webtrans also uses [[gwt-dispatch|http://code.google.com/p/gwt-dispatch/]], [[google-guice|http://code.google.com/p/google-guice/]], [[gwt-presenter|http://code.google.com/p/gwt-presenter/]], [[gwteventservice|http://code.google.com/p/gwteventservice/]] and [[gwt-log|http://code.google.com/p/gwt-log/]].


# Model-View-Presenter Pattern (MVP)
Webtrans uses the MVP software pattern for most of its client-side functionality. 

![MVP in Webtrans](http://zanata.org/images/diagrams/zanata-2.0-architecture-webtrans-mvp.svg)