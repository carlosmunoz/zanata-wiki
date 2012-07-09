# Overview of the Python library for communicating with Flies

# Introduction

The flies-python-library provides project and iteration services for creating projects or iterations, getting a list of projects on a Flies server, and retrieving info for a single project or single iteration. It also provides a document service for pulling or pushing the content to/from a Flies server.

# Getting the Code

See instructions for the [[Zanata Python Client]]

# Install

flies-python-library is installed with flies-python-client. You can look through the [[Flies Python Client]] instructions for details.

# Configuration

flies-python-library uses the same configuration files as [[Flies Python Client]]

# API of flies python library

You can find detailed information about the API of flies-python-library in http://jamesni.fedorapeople.org/py-client-docs/

# Use flies python library

You can "import fliesclient.flieslib" in you program and use the FliesResource class to create a resource instance, which includes the Project Service and Document service. Then you can access a project, iteration or document by manipulating this resource instance. Here's a short program to print a list of projects:

       import fliesclient.flieslib
    
       # URL of Flies server
       server = "url of Flies Server"
    
       # Authenticate using your user name and api key on Flies Server.
       user = "admin"
       apikey = "api key of admin"
    
       # Create a FliesResource instance which will call project service and document service.
       client = fliesclient.flieslib.FliesResource(server, user, apikey)
    
       # Query the Flies server for a list of all projects.
       project_list = client.projectservice.list()