This page lists all security vulnerabilities fixed in zanata. Each vulnerability is assigned a security impact rating on a four-point scale (low, moderate, important and critical). The versions that are affected by each vulnerability are also listed. 

## [CRITICAL] CVE-2013-2165 JBoss RichFaces: Remote code execution due to insecure deserialization

### Description

A flaw was found in the way RichFaces ResourceBuilderImpl handled deserialization. A remote attacker could use this flaw to trigger the execution of the deserialization methods in any serializable class deployed on the server. This could lead to a variety of security impacts depending on the deserialization logic of these classes. The fix for this issue introduces a whitelist to limit classes that can be deserialized by RichFaces.

### Affected versions

3.0.0-3.1.2

### Patch commit(s)

https://github.com/zanata/zanata-server/commit/fe0c9cac69da2420eb749d7d23437db78fab81da


## [CRITICAL] CVE-2013-4486 Zanata: Remote code execution due to EL interpolation in logging

### Description

Seam logging evaluates expression language (EL) statements in log messages. If an application includes user-provided strings in log messages directly via string concatenation, then a remote attacker could inject EL statements directly into the log messages, which would be evaluated on the server. If debug logging is enabled, Zanata performs logging of user-supplied strings using string concatenation. A remote attacker could use this flaw to execute arbitrary code in the context of the application server running Zanata.

### Affected versions

3.0.0-3.1.2

### Patch commit(s)

https://github.com/zanata/zanata-server/commit/c4152ef90609367c870618aee8d49c302619cc40