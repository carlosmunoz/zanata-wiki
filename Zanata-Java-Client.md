# Java command line client (unsupported!)

## Java command line client (unsupported!)

Example command lines:
    $ zanataj --help
    
    Usage: zanataj [OPTION]... <command> [COMMANDOPTION]...
    Zanata command-line client
    
     <command>          : Command name
     --debug (-X)       : Enable debug logging
     --errors (-e)      : Output full execution error messages (stacktraces)
     --help (-h, -help) : Display this help and exit
     --quiet (-q)       : Quiet mode - error messages only
     --version (-v)     : Output version information and exit
    
    Type 'zanataj help <command>' for help on a specific command.
    
    Available commands:
      listremote
      publican-push
      putproject
      putuser
      putversion
    
    $ zanataj help putproject
    
    Usage: putproject [options]
    Creates or updates a Zanata project.
    
     --debug (-X)        : Enable debug logging
     --errors (-e)       : Output full execution error messages (stacktraces)
     --help (-h, -help)  : Display this help and exit
     --key KEY           : Zanata API key (from Zanata Profile page)
     --project-desc DESC : Zanata project description
     --project-name NAME : Zanata project name
     --project-slug PROJ : Zanata project slug/ID
     --quiet (-q)        : Quiet mode - error messages only
     --url URL           : Zanata base URL, eg http://example.com/zanata/
     --user-config FILE  : Zanata user configuration, eg /home/user/.config/zanata.ini
     --username USER     : Zanata user name
    
    $ zanataj listremote --help
    
    Usage: listremote [options]
    Lists all remote documents in the configured Zanata project version.
    
     --debug (-X)              : Enable debug logging
     --errors (-e)             : Output full execution error messages (stacktraces)
     --help (-h, -help)        : Display this help and exit
     --key KEY                 : Zanata API key (from Zanata Profile page)
     --project PROJ            : Zanata project ID/slug.  This value is required unless specified in zanata.xml.
     --project-config FILENAME : Zanata project configuration, eg zanata.xml
     --project-version VER     : Zanata project version ID  This value is required unless specified in zanata.xml.
     --quiet (-q)              : Quiet mode - error messages only
     --url URL                 : Zanata base URL, eg http://example.com/zanata/
     --user-config FILE        : Zanata user configuration, eg /home/user/.config/zanata.ini
     --username USER           : Zanata user name