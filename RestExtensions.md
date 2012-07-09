# REST Extension examples

# For Resource and `ResourceMeta`

Available Extension Object: `PoHeader` (extension: `gettext`)

Field: String comment
Field: List(`HeaderEntry`) entries

For example:
`ResourceMeta` with `PoHeader` extension(comment and entries)
    {"name":"id",
    "contentType":"text/plain",
    "lang":"en-US",
    "extensions":[{"object-type":"po-header","comment":"comment_value",
    "entries":[{"key":"h1","value":"v1"},{"key":"h2","value":"v2"}]}],
    "type":"FILE"}

for example, the `getttext` header for file id.pot
    # comment_value
    msgid ""
    msgstr ""
    "h1: v1\n"
    "h2: v2\n"


# For `TextFlow`

Available Extension Objects:`PotEntryHeader` (extension: `gettext`) and `SimpleComment` (extension: comment)

`PotEntryHeader`

Field:String context
Field:List(String) flags
Field:List(String) references

`SimpleComment`

Field:String value
Field:String space(preserved)

For example:

`TextFlow` with `SimpleComment`(value="extracted comment" and space="preserve") and `PotEntryHeader`(context="context_value" and reference=[and flags=["java-format"]("fff"]))
    {"lang":"en-US",
    "content":"ttff",
    "extensions":[{"object-type":"comment","value":"extracted comment","space":"preserve"},
    {"object-type":"pot-entry-header","context":"context","references":["fff"],"flags":["java-format"]}]}

in the pot file:

    # ignored comment (not supported by Flies)
    #. extracted comment
    #, context_value
    #: fff
    #, java-format
    msgid "ttff"
    msgstr ""

and in the corresponding ja.po file
    # translator comment (see TextFlowTarget/SimpleComment below)
    #. extracted comment
    #, context_value
    #: fff
    #, java-format
    msgid "ttff"
    msgstr "some translation"

# For `TextFlowTarget`

Available Extension Object:`SimpleComment` (extension: comment)

`SimpleComment`

Field:String value
Field:String space(preserved)

For example:
`TextFlowTarget` with `SimpleComment`
    {"state":"Approved",
    "translator":{"email":"id","name":"name"},
    "content":"some translation",
    "extensions":[{"object-type":"comment","value":"translator comment","space":"preserve"}]}

    # translator comment
    #. extracted comment
    #, context_value
    #: fff
    #, java-format
    msgid "ttff"
    msgstr "some translation"


# For `TranslationsResource`

Available Extension Object:`PoTargetHeader` (extension: `gettext`)

Field: String comment
Field: List(`HeaderEntry`) entries

For example:

`TranslationsResource` with `PoTargetHeader`()
    {"extensions":[{"object-type":"po-target-header",
    "comment":"target header comment",
    "entries":[{"key":"ht","value":"vt1"},{"key":"th2","value":"tv2"}]}],
    "textFlowTargets":[{"resId":"rest1","state":"Approved","translator":{"email":"root@localhost","name":"Admin user"},"content":"hello world"}]}

for example, the `getttext` header for file ja.po

    # target header comment
    msgid ""
    msgstr ""
    "ht: vt1\n"
    "th2: tv2\n"