# Integrating with external authentication via JAAS

# Introduction

## Configure Zanata security 
Open `$JBOSS_HOME/standalone/configuration/standalone.xml` and look for the `naming` subsystem. Add the following sections to the `bindings` section:

```xml
<subsystem xmlns="urn:jboss:domain:naming:1.3">
    ...
    <bindings>
        <simple name="java:global/zanata/security/auth-policy-names/<aythtype>" value="<policy-name>"/>
        <simple name="java:global/zanata/security/admin-users" value="<list of usernames>"/>
    </bindings>
    ...
</subsystem>
```

... where `<authtype>` is the authentication type enabled for the Zanata instance.
Accepted values are: internal, jaas, openid, kerberos

... and `<policy-name>` is the authentication policy name as defined in standalone.xml (see sections below).

In most cases, only a single authentication mechanism should be active at any given time, and Zanata will refuse to start if these settings are incorrect. However, the 'internal' and 'openid' mechanisms can be enabled simultaneously.

1. After first login, you will be able to make yourself an admin in the database by running the following database scripts:

```sql
 insert into HAccountMembership(accountId, memberOf) values((select id from HAccount where username = 'myusername'), (select id from HAccountRole where name = 'admin'));
```

- Check current membership using:

```sql
SELECT r.name FROM HAccountRole AS r, HAccountMembership AS m WHERE m.accountId = (SELECT id FROM HAccount WHERE username = 'myusername') AND m.memberOf = r.id;
```

2. It is possible to define admin users in the zanata.properties file:

```properties
zanata.security.admin.users = user1,user2
```

The property above must contain a comma-separated list of user names. Zanata will check that these users have administrator privileges every time they log in, or in case the user doesn't yet exist, when they are created. This feature is recommended for the first time Zanata is started, and to avoid being locked out of the system at any time. However it is not meant to be used to manage admin users system wide.

Use the 'register' page to add the admin users, with user names exactly matching the names in the zanata.properties file. The accounts will have to be activated using the activation links in the activation emails sent during the registration process before login is possible. If there is an issue with delivery of activation emails, accounts can be activated manually using:

```sql
UPDATE HAccount SET enabled = true WHERE username = 'myusername';
```

# Examples

## Internal Authentication

Make sure zanata.properties has the authentication property `zanata.security.auth.policy.internal=zanata.internal` (Note the value of the property matches the application policy name below).

standalone.xml

```xml
      ...
      <application-policy name="zanata.internal">
         <authentication>
            <login-module
               code="org.jboss.seam.security.jaas.SeamLoginModule"
               flag="required">
            </login-module>
         </authentication>
      </application-policy>
      ...
```

## Pure JAAS

Make sure zanata.properties has the authentication property `zanata.security.auth.policy.jaas=zanata.jaas` (Note the value of the property matches the application policy name below).

eg DatabaseServerLoginModule (you'll need to deploy a datasource too)

standalone.xml:

```xml
      ...
      <application-policy name="zanata.jaas">
        <authentication>
            <login-module
    code="org.jboss.security.auth.spi.DatabaseServerLoginModule"
    flag="required">
            <module-option name="dsJndiName" value="java:authdb" />
            <module-option name="principalsQuery" value="SELECT password FROM users WHERE username = ?" />
            <module-option name="rolesQuery" value="select '','' FROM users WHERE username = ?" />
            <module-option name="hashAlgorithm" value="md5" />
            <module-option name="hashEncoding" value="hex" />
            </login-module>
        </authentication>
      </application-policy>
      ...
```

## Kerberos/SPNEGO

Make sure zanata.properties has the authentication property `zanata.security.auth.policy.kerberos=zanata.kerberos` (Note the value of the property matches the application policy name below).

**Configure Kerberos**

Kerberos configuration is located at /etc/krb5.conf
A kerberos keytab should be obtanined for HTTP and the server where the applications is being deployed.

Make sure you use OpenJDK, not Oracle/Sun JDK, unless you have the [appropriate JCE policy file ](http://www.oracle.com/technetwork/java/javase/downloads/jce-6-download-429243.html) (untested).


**Application Descriptors**

standalone.xml:

```xml
      ...
      <application-policy name="zanata.kerberos">
        <authentication>
          <login-module code="org.jboss.security.negotiation.spnego.SPNEGOLoginModule" flag="optional">
            <module-option name="password-stacking" value="useFirstPass" />
            <module-option name="serverSecurityDomain" value="host" />
            <module-option name="removeRealmFromPrincipal" value="true" />
          </login-module>
          <login-module code="com.sun.security.auth.module.Krb5LoginModule" flag="optional">
            <module-option name="storePass" value="true" />
          </login-module>
        </authentication>
      </application-policy>
    
      <application-policy name="host">
        <authentication>
          <login-module code="com.sun.security.auth.module.Krb5LoginModule" flag="required">
            <module-option name="storeKey" value="true" />
            <module-option name="useKeyTab" value="true" />
            <module-option name="principal" value="HTTP/hostname@KERBEROS.DOMAIN" />
            <module-option name="keyTab" value="/etc/jboss.keytab" />
            <module-option name="doNotPrompt" value="true" />
          </login-module>
        </authentication>
      </application-policy>
      ...
```
    

The principal and keyTab attributes in the example above should be replaced with appropriate values for the principal as stored in the keytab and the location of the keytab file.

## OpenID

Make sure zanata.properties has the authentication property `zanata.security.auth.policy.openid=zanata.openid` (Note the value of the property matches the application policy name below).

standalone.xml:

```xml
      ...
      <application-policy name="zanata.openid">
         <authentication>
           <login-module code="org.zanata.security.OpenIdLoginModule"
             flag="required">
           </login-module>
         </authentication>
       </application-policy>
      ...
```