# User Guide of Flies-Publican client.

# Introduction

Because flies-publican is at fast pace of development, the followings might not always correspond to the latest version.

# Usage

    $ flies-publican [--help] [upload|download] [options]*

## Special Option

- --help - Prints help message.

## Subcommands

- upload - Uploads po/pot files to Flies server.

- download - Downloads po/pot files from Flies server.

## Subcommand options

- --user USER - Inputs USER as username of connection to Flies server.

- --key KEY - Inputs KEY as apikey of connection to Flies server.

- --src SRC - Inputs SRC as source of data. With "upload" this takes local location of Publican standardized PO/POT repository. With "download" this takes remote namespace of Flies server.

- --dst DST - Inputs DST as destination of data. With "upload" this takes remote namespace of Flies server. With "download" this takes local location of Publican standardized PO/POT repository.

- --debug - Outputs debug information.

- --importpo - Replaces existing Flies translations with contents of existing PO files - VERY DANGEROUS.  (Applies only to 'upload')

- --exportpot - Generates POT files from Flies database - rarely used.  (Applies only to 'download')