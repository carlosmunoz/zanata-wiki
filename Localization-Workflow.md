# Localization workflow with Zanata as translation tool.

# Requirements

- Corresponding version control system client-side command.
- Publican.
- zanata-python-client.
- Network connection to Zanata server.

# Workflows

- Preparation
- Translation

## Preparation

1. Check out publican files from repository to local directory.
1. Update POT files:

    $ publican update_pot

1. Update PO files:

    $ publican update_po --langs=all

1. Check that the localized version is building properly (assume Spanish):

    $ publican build --formats=html --langs=es-ES # for local
    $ publican package --brew --langs=es-ES # for real brew

## Translation

1. FIXME: Import PO/POT files to Zanata server:

    $ flies-publican uploadpo 

1. Translate on Flies UI.
1. FIXME: Export PO/POT flies from Zanata server:

    $ flies-publican downloadpo

1. Check HTML book are built properly (assume Spanish):

    $ publican build --formats=html --langs=es-ES

1. Proofread in Firefox.
1. Brew document to respository:

    $ publican package --brew --langs=es-ES