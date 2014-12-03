Make sure your jboss standalone.xml has management native socket binding enabled.
You should see an entry like this:

    <socket-binding name="management-native" interface="management"
      port="${jboss.management.native.port,env.JBOSS_MANAGEMENT_NATIVE_PORT:9999}" />

Once you have management socking binding open, download the [messaging config file]( https://raw.githubusercontent.com/zanata/zanata-server/rhbz1120457-email-queue/etc/scripts/standalone.cli.messaging.config).

> This config file has been tested with EAP6 and Wildfly8.1.0-FINAL

Run 

    jboss-cli.sh --file=path/to/standalone.cli.messaging.config

Verify the result is successful.

Restart your server.