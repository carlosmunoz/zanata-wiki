# Integrating with external authentication via JAAS

# Introduction

1. Edit `zanata.war/WEB-INF/classes/META-INF/components.xml` to deactivate internalauthentication and enable your preferred module.
1. Specify your preferred login module in `$JBOSS_HOME/server/<profile>/conf/login-config.xml`.  You will need an application-policy with the name `"zanata"`.
1. After first login, you will need to make yourself an admin in the database:
    mysql> insert into HAccountMembership(accountId, memberOf) values((select id from HAccount where username = 'myusername'), (select id from HAccountRole where name = 'admin'));
- Check current membership using:
    mysql> SELECT r.name FROM HAccountRole AS r, HAccountMembership AS m WHERE m.accountId = (SELECT id FROM HAccount WHERE username = 'myusername') AND m.memberOf = r.id;
1. **SECURITY**: Zanata versions < 1.4 automatically create an internal user `admin` which must be disabled or deleted for security. To disable, visit the page `Administration/Manage Users` (as an administrator), click `Edit` on the line for `admin`, deselect `Account enabled`, then `Save`.

# Examples

## Pure JAAS

eg DatabaseServerLoginModule (you'll need to deploy a datasource too)

login-config.xml:
      <application-policy name="zanata">
        <authentication>
            <login-module
    code="org.jboss.security.auth.spi.DatabaseServerLoginModule"
    flag="required">
            <module-option name="dsJndiName">java:authdb</module-option>
            <module-option name="principalsQuery">SELECT password FROM users WHERE username = ?</module-option>
            <module-option name="rolesQuery">select '','' FROM users WHERE username = ?</module-option>
            <module-option name="hashAlgorithm">md5</module-option>
            <module-option name="hashEncoding">hex</module-option>
            </login-module>
        </authentication>
      </application-policy>
    

## Kerberos/SPNEGO

**Configure Kerberos**

Kerberos configuration is located at /etc/krb5.conf
A kerberos keytab should be obtanined for HTTP and the server where the applications is being deployed.

**Add a SPNEGO authenticator to JBoss**

In `$JBOSS_HOME/server/<profile>/deployers/jbossweb.deployer/META-INF/war-deployers-jboss-beans.xml`, remove SECURITY_DOMAIN if present, and add the following entry under the authenticators' section:
         <entry>
             <key>SPNEGO</key>
             <value>org.jboss.security.negotiation.NegotiationWithBasicFallbackAuthenticator</value>
         </entry>

**JBoss Negotiation Libraries**

Add the jboss-negotiation.jar file to `$JBOSS_HOME/server/<profile>/lib`

Note: we use jboss-negotiation compiled from SVN tag 2.0.3.SP3, plus the patches in https://issues.jboss.org/browse/SECURITY-448.

Make sure you use OpenJDK, not Oracle/Sun JDK, unless you have the [appropriate JCE policy file ](http://www.oracle.com/technetwork/java/javase/downloads/jce-6-download-429243.html) (untested).


**Application Descriptors**

login-config.xml:
      <application-policy name="zanata">
        <authentication>
          <login-module code="org.jboss.security.negotiation.spnego.SPNEGOLoginModule" flag="optional">
            <module-option name="password-stacking">useFirstPass</module-option>
            <module-option name="serverSecurityDomain">host</module-option>
            <module-option name="removeRealmFromPrincipal">true</module-option>
          </login-module>
          <login-module code="com.sun.security.auth.module.Krb5LoginModule" flag="optional">
            <module-option name="storePass">true</module-option>
          </login-module>
          <login-module code="org.jboss.security.auth.spi.UsersRolesLoginModule" flag="required">
            <!-- property files can found under conf/props directory -->
            <module-option name="password-stacking">useFirstPass</module-option>
            <module-option name="usersProperties">props/spnego-users.properties</module-option>
            <module-option name="rolesProperties">props/spnego-roles.properties</module-option>
          </login-module>
        </authentication>
      </application-policy>
    
      <application-policy name="host">
        <authentication>
          <login-module code="com.sun.security.auth.module.Krb5LoginModule" flag="required">
            <module-option name="storeKey">true</module-option>
            <module-option name="useKeyTab">true</module-option>
            <module-option name="principal">HTTP/hostname@KERBEROS.DOMAIN</module-option>
            <module-option name="keyTab">/etc/jboss.keytab</module-option>
            <module-option name="doNotPrompt">true</module-option>
          </login-module>
        </authentication>
      </application-policy>
    

The principal and keyTab attributes in the example above should be replaced with appropriate values for the principal as stored in the keytab and the location of the keytab file.
Make sure both the spnego-users.properties and spnego-roles.properties are empty files under `$JBOSS_HOME/server/<profile>/deploy/conf`.

## Fedora Account System (OpenID)

login-config.xml:
      <application-policy name="zanata">
         <authentication>
           <login-module code="org.zanata.security.OpenIdLoginModule"
             flag="required">
           </login-module>
         </authentication>
       </application-policy>