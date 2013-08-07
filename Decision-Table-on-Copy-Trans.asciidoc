The behavior should be similar to client pull, that is, don't overwrite unless either completed (Translated or Approved) or the current state is untranslated.

We nolonger copy approved states.

[format="csv",frame="topbot",options="header"]
[cols="4*,s,s"]
|====
Current State,State (Other Version),Mismatch Detected,Mismatch Resolution,CopyTrans,Final State
Untranslated,Untranslated,-,-,No,Untranslated
Untranslated,Fuzzy,-,-,No,Untranslated
Untranslated,Rejected,-,-,No,Untranslated
Untranslated,Translated,Yes,Discard,No,Untranslated
Untranslated,Translated,Yes,Down to Fuzzy,Yes,Fuzzy
Untranslated,Translated,Yes,As Translated,Yes,Translated
Untranslated,Translated,No,-,Yes,Translated
Untranslated,Approved,Yes,Discard,No,Untranslated
Untranslated,Approved,Yes,Down to Fuzzy,Yes,Fuzzy
Untranslated,Approved,Yes,As Translated,Yes,Translated
Untranslated,Approved,No,-,Yes,Approved
Fuzzy,Untranslated,-,-,No,Fuzzy
Fuzzy,Fuzzy,-,-,No,Fuzzy
Fuzzy,Reject,-,-,No,Fuzzy
Fuzzy,Translated,Yes,Discard,No,Fuzzy
Fuzzy,Translated,Yes,Down to Fuzzy,Yes,Fuzzy
Fuzzy,Translated,Yes,As Translated,Yes,Translated
Fuzzy,Translated,No,-,Yes,Translated
Fuzzy,Approved,Yes,Discard,No,Fuzzy
Fuzzy,Approved,Yes,Down to Fuzzy,Yes,Fuzzy
Fuzzy,Approved,Yes,As Translated,Yes,Translated
Fuzzy,Approved,No,-,Yes,Approved
Reject,Untranslated,-,-,No,Reject
Reject,Fuzzy,-,-,No,Reject
Reject,Reject,-,-,No,Reject
Reject,Translated,Yes,Discard,No,Reject
Reject,Translated,Yes,Down to Fuzzy,Yes,Fuzzy
Reject,Translated,Yes,As Translated,Yes,Translated
Reject,Translated,No,-,Yes,Translated
Reject,Approved,Yes,Discard,No,Reject
Reject,Approved,Yes,Down to Fuzzy,Yes,Fuzzy
Reject,Approved,Yes,As Translated,Yes,Translated
Reject,Approved,No,-,Yes,Approved
Traslated,-,-,-,No,Translated
Approved,-,-,-,No,Approved
|====

