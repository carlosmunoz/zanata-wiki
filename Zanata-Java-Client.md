# Java command line client (now available as zip/tar distribution!)

## Java command line client (now available as zip/tar distribution!)

Example command lines:
    $ zanata-cli --help
    
    Usage: zanata-cli [OPTION]... <command> [COMMANDOPTION]...
    Zanata Java command-line client

     <command>          : Command name
     --debug (-X)       : Enable debug logging
     --errors (-e)      : Output full execution error messages (stacktraces)
     --help (-h, -help) : Display this help and exit
     --quiet (-q)       : Quiet mode - error messages only
     --version (-v)     : Output version information and exit

    Type 'zanata-cli help <command>' for help on a specific command.

    Available commands:
      list-remote
      pull
      push
      put-project
      put-user
      put-version
      stats

## Detailed help for individual command

    $ zanata-cli --help pull

## bash (shell) auto completion
In the zip/tar distribution, once unzipped, you will find a script called zanata-cli-completion under bin/ folder. If you are using bash as your default shell, or if you are keen to use auto completion

    $ echo $SHELL

will tell you what shell you are using. You can always type bash to switch to BA shell.
Once you are in bash, type

    $ . zanata-cli-completion

This will source the auto completion script in current shell session. You will then be able to press tab to activate auto completion.

    $ zanata-cli [tab] [tab]
     --help       list-remote  pull         push         put-project  put-user     put-version  stats
    $ zanata-cli pul [tab]
    $ zanata-cli pull
    $ zanata-cli pull [tab] [tab]
     -B                  --encode-tabs       --include-fuzzy     --project-config    --quiet ...

Note: bash completion script can be put under /etc/bash_completion.d/, then it will be available to all bash session all the time(no need to source it every time). BUT, this script is generated at build time. As zip distribution, it's up to you to manage the installation and update. Once we have RPM ready, it will all be handled automatically.

## Available as RPM package
We are working on getting it into Fedora first. All the dependent packages are either in review or ready for review state. 