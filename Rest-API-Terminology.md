# Translations of REST api terms

These DTOs all provide different views of a document (HDocument in our domain model):

<table>
  <tr><td>XML element</td><td>Java DTO type</td><td>Description</td></tr>
  <tr><td>resource-meta</td><td>ResourceMeta</td><td>document:  metadata and Extensions only, no content</td></tr>
  <tr><td>resource</td><td>Resource</td><td>source document: source text (TextFlows), metadata and Extensions</td></tr>
  <tr><td>translations</td><td>TranslationsResource</td><td>translated document: target text (TextFlowTargets) and Extensions, but not the source document metadata</td></tr>
</table>

Supported extensions for PUT/POST operations:
    Resource/ResourceMeta: PoHeader
    TranslationsResource: PoTargetHeader
    nothing??: PoTargetHeaders -> PoTargetHeaderEntry
    nothing??: PotEntryHeader
    TextFlowTarget: SimpleComment

NB: There doesn't appear to be any live, referenced code which processes the extensions PotEntryHeader, PoTargetHeaders, PoTargetHeaderEntry.  And SimpleComments are only handled for TextFlowTarget, not TextFlow.


# URL Patterns

## Hierarchy

    /p/sample-project/v/1.0/r/mydoc.txt (document level)
    /p/sample-project/v/1.0/r/mydoc.txt/metadata (document level)
    /p/sample-project/v/1.0/r/mydoc.txt/translations/en-AU (document level)
    /p/sample-project/v/1.0/r/mydoc.txt/translations/en-AU/metadata (document level)

## Resource Level

    /r/ GET returns list of resources
    /r/ POST adds resource
    /r/{id} GET returns specific resource
    /r/{id}/meta GET returns specific resource
    /r/{id}/translations

## Future Extensions

### Access to individual TextFlows and Translations

Source:
    /p/sample-project/v/1.0/r/mydoc.txt/content/tf1

Translations:
    /p/sample-project/v/1.0/r/mydoc.txt/content/tf1/translations/en-AU