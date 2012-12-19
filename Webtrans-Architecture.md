The Translation Editor (Webtrans) is a GWT application. It is distributed from a Zanata server and runs in a browser (Chrome and Firefox are targeted specifically), and uses GWT-RPC to communicate with the server.

The GWT SDK is required to develop this component.

![Webtrans general architecture](http://zanata.org/images/diagrams/zanata-2.0-architecture-webtrans.svg)

As well as core GWT libraries, webtrans also uses [[gwt-dispatch|http://code.google.com/p/gwt-dispatch/]], [[google-guice|http://code.google.com/p/google-guice/]], [[gwt-presenter|http://code.google.com/p/gwt-presenter/]], [[gwteventservice|http://code.google.com/p/gwteventservice/]] and [[gwt-log|http://code.google.com/p/gwt-log/]].

# Model-View-Presenter Pattern (MVP)
Webtrans uses the MVP software pattern for most of its client-side functionality. In MVP, the Presenter is the main controlling component and performs all interactions with the model, with the view being a lightweight display component with as little control and data manipulation logic as possible.

Zanata uses the gwt-presenter library to handle the connections between the components of MVP. The diagram below illustrates how MVP is approached in Zanata.

![MVP in Webtrans](http://zanata.org/images/diagrams/zanata-2.0-architecture-webtrans-mvp.svg)

For each part of Webtrans there is a Presenter component, a Display interface, and a View component that implements the Display interface. These components usually share a prefix, and use the component type as a suffix, e.g. the document list is handled by DocumentListPresenter, DocumentListDisplay and DocumentListView. In some cases the Display interface is defined within the presenter class, e.g. the project-wide search page is handled by SearchResultsPresenter, SearchResultsPresenter.Display and SearchResultsView. These components are associated with each other in org.zanata.webtrans.client.gin.WebTransClientModule, allowing the View to be injected in the Presenter's constructor.

The Model part of MVP in Webtrans consists largely of calls to the server (using gwt-rpc) to retrieve data, and client-side caches of this data in the form of fields in the presenter. e.g. the document list is retrieved using the GetDocumentList RPC method (in this case initiated in Application.java), and stored in DocumentListPresenter as the HashMap 'idsByPath'.

# GWT-RPC

The diagram below illustrates how gwt-rpc is used in Zanata.

![GWT RPC in Webtrans](http://zanata.org/images/diagrams/zanata-2.0-architecture-webtrans-rpc.svg)

A gwt-rpc call requires a Result, an Action, an Action Handler and an Async Callback, all with appropriate relationships. e.g. retrieval of a list of documents requires GetDocumentListResult (implements DispatchResult), GetDocumentList (extends AbstractWorkspaceAction<GetDocumentListResult>), GetDocumentListHandler (extends AbstractActionHandler<GetDocumentList, GetDocumentListResult>), and an anonymous callback object (new AsyncCallback<GetDocumentListResult>() {...}).

In a Presenter class, Action and AsyncCallback objects are instantiated and passed as arguments to the execute(...) method of an injected org.zanata.webtrans.client.rpc.CachingDispatchAsync (Dispatcher). The Dispatcher handles communication with the server, invocation of the appropriate ActionHandler on the server (specified in the @ActionHandlerFor(...) annotation in the ActionHandler) and invocation of the onSuccess(...) or onFailure(...) method of the AsyncCallback.

# Presenter and View Relationships

Webtrans consists of a nested set of Presenters, and a similarly nested set of Views. Its entry point is org.zanata.webtrans.client.Application, retrieves the top-level view (AppView) and adds it to the page. Other views are nested within AppView as shown in the diagram below.

![Webtrans View/Presenter hierarchy](http://zanata.org/images/diagrams/zanata-2.0-webtrans-hierarchy.svg)

The diagrams below show the arrangement of views in the Webtrans UI on the main UI tabs (doclist, editor, search).

[![Webtrans Views (doclist)](http://zanata.org/images/diagrams/zanata-2.0-webtrans-views-doclist-small.png)](http://zanata.org/images/diagrams/zanata-2.0-webtrans-views-doclist.svg "Click to view full size")

[![Webtrans Views (editor)](http://zanata.org/images/diagrams/zanata-2.0-webtrans-views-editor-small.png)](http://zanata.org/images/diagrams/zanata-2.0-webtrans-views-editor.svg "Click to view full size")

[![Webtrans Views (search)](http://zanata.org/images/diagrams/zanata-2.0-webtrans-views-search-small.png)](http://zanata.org/images/diagrams/zanata-2.0-webtrans-views-search.svg "Click to view full size")