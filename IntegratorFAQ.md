# FAQ about integrating with flies


# Why doesn't flies communicate directly with version control systems such as SVN and Git?

A typical work flow in open source L10N has been to store the translation resources (i.e. PO files) within a version control system (VCS) such as CVS, SVN or Git. Applications such as Transifex has made life a lot easier for translators, as they can retrieve and submit files via a web interface, and these are automatically syncronized with the upstream VCS. With this approach, translations are always kept 'upstream'.

With flies we have moved away from this approach to translation work flow, to an approach where the external 'content provider' is responsible for publishing (or pushing) content for translations, and similarly resposible for retrieving (or pulling) translated content. This approach is very similar to how VCSs work, and flies is indeed a VCS specifically for translations. This means that flies tracks changes to translation units, and as the project evolves maintains a much richer representation than what the typical L10N formats such as PO can store. 

When you push content to flies for translation, you are not actually pushing PO files or other translation formats, but a flies representation of the document. Flies exposes a RESTful API for publishing and retrieving content, where content providers can push or pull Json or XML representations of the content. As long as the content provider can convert to and from the original format to the flies representation, you can translate the content in flies.

With this in mind, it doesn't really make sense to syncronize directly with other VCSs. The flies approach encourages explicit push and pull of content, and we provide libraries and client tools to help with this tasks for common file formats and project types. 

## That said, how do I hook flies up directly with upstream VCSs?

Note that with the flies approach it is technically possible to use the same work flow as systems that do communicate directly with upstream VCSs (e.g. transifex), although we don't recommmend this approach for the reasons given above. 

To accomplish a tight VCS integration, you need to use an agent to facilitate the communication between flies and the upstream VCS. This agent would listen to changes in the upstream VCS using polling or push-notifications (if available), and would push upstream changes to flies immediately. Similarly, the agent would listen to changes from flies using polling, and again commit these changes upstream. If there is sufficient demand, a push-based notification system could trivially be added to flies.

TODO: Diagram: VCS <> agent <> Flies

Having an agent facilitate the VCS syncronization - as opposed to doing the syncronization within flies - is also a more secure and robust way of handling VCS syncronization than other approaches. With the agent-approach, the agent could live 'upstream' and the syncronization could happen in the security domain of the upstream project. Contrast this approach with e.g. transifex where the central transifex application needs security credentials for all projects stored locally external to the upstream project to accomplish the same job. In adition, having the agent externally also allows for a greater flexibility in syncronization work flow, as the upstream project can themselves decide on syncronization policies.