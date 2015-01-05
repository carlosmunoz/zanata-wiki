# FAQ about integrating with Zanata


# Why doesn't Zanata communicate directly with version control systems such as SVN and Git?

A typical work flow in open source L10N has been to store the translation resources (i.e. PO files) within a version control system (VCS) such as CVS, SVN or Git. Applications such as Transifex has made life a lot easier for translators, as they can retrieve and submit files via a web interface, and these are automatically synchronised with the upstream VCS. With this approach, translations are always kept 'upstream'.

With Zanata we have moved away from this approach to translation work flow, to an approach where the external 'content provider' is responsible for publishing (or pushing) content for translations, and similarly resposible for retrieving (or pulling) translated content. This approach is very similar to how VCSs work, and Zanata is indeed a VCS specifically for translations. This means that Zanata tracks changes to translation units, and as the project evolves maintains a much richer representation than what the typical L10N formats such as PO can store. 

When you push content to Zanata for translation, you are not actually pushing PO files or other translation formats, but a Zanata representation of the document. Zanata exposes a RESTful API for publishing and retrieving content, where content providers can push or pull Json or XML representations of the content. As long as the content provider can convert to and from the original format to the Zanata representation, you can translate the content in Zanata.

With this in mind, it doesn't really make sense to synchronise directly with other VCSs. The Zanata approach encourages explicit push and pull of content, and we provide libraries and client tools to help with this tasks for common file formats and project types. 

## That said, how do I hook Zanata up directly with upstream VCSs?

Note that with the Zanata approach it is technically possible to use the same work flow as systems that do communicate directly with upstream VCSs (e.g. transifex), although we don't recommmend this approach for the reasons given above. 

To accomplish a tight VCS integration, you need to use an agent to facilitate the communication between flies and the upstream VCS. This agent would listen to changes in the upstream VCS using polling or push-notifications (if available), and would push upstream changes to Zanata immediately. Similarly, the agent would listen to changes from flies using polling, and again commit these changes upstream. If there is sufficient demand, a push-based notification system could trivially be added to Zanata.

TODO: Diagram: VCS <> agent <> Zanata

Having an agent facilitate the VCS synchronisation - as opposed to doing the synchronisation within Zanata - is also a more secure and robust way of handling VCS synchronisation than other approaches. With the agent-approach, the agent could live 'upstream' and the synchronisation could happen in the security domain of the upstream project. Contrast this approach with e.g. transifex where the central transifex application needs security credentials for all projects stored locally external to the upstream project to accomplish the same job. In adition, having the agent externally also allows for a greater flexibility in synchronisation work flow, as the upstream project can themselves decide on synchronisation policies.