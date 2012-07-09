# REST Project Resources

# Working with Projects

Note that these URLs are under the Base URL.  See [RestApiV1].

## Retrieving list of Projects

**Url:** `/projects/`

**Verb:** `GET`

**Media Types:**
- `application/json`
- `application/vnd.flies.projects+xml`

**Requires Authentication:** No

### Example (json)

**Request:**

    GET /projects/
    Host: www.example.com
    Accept: application/json

**Response:**

    HTTP/1.1 200 OK
    Content-Type: application/json
    Content-Length: 1445
    
    [ {
        "name" : "Sample Project",
        "id" : "sample-project",
        "description" : "An example Project",
        "type" : "IterationProject"
        "links" : [ ]
      } 
    ]
    
    

### Example (XML)

**Request:**

    GET /projects/
    Host: www.example.com
    Accept: application/vnd.flies.projects+xml

**Response:**

    HTTP/1.1 200 OK
    Content-Type: application/vnd.flies.projects+xml
    Content-Length: 1445
    
    <?xml version="1.0" encoding="UTF-8"?>
    <projects xmlns="http://flies.fedorahosted.org/api/v1/">
        <project revision="1" id="jbt">
            <name>JBoss Tools</name>
            <description>JBoss Tools</description>
            <project-iterations/>
        </project>
        <project revision="1" id="JBoss_SOA_Platform-5.0.0-Release_Notes">
            <name>JBoss_SOA_Platform-Release_Notes</name>
            <description>JBoss_SOA_Platform-Release_Notes</description>
            <project-iterations/>
        </project>
    </projects>
    

# Working with a single Project

## Retrieving a Project

**Request:**

    GET /projects/p/{project_id}
    Host: www.example.com
    Accept: application/json

**Response:**

    HTTP/1.1 200 OK
    Content-Type: application/json
    Content-Length: 1445
    
    {
        "name" : "Sample Project",
        "id" : "sample-project",
        "description" : "An example Project",
        "type" : "IterationProject"
        "iterations": "[{'name':'Fedora 13','id':'f13','revision':1,
                       'summary':'Fedora13','links':[]},
                       {'name':'Fedora 14','id':'f14','revision':1,
                       'summary':'blah','links':[]}]"
        "links":[]
    }

## Create / Updating a Project

**Request:**

    PUT /projects/p/{project_id}
    Host: www.example.com
    Accept: application/json
    X-Auth-User: admin
    X-Auth-Token: 12345678901234567890123456789012
    Body: {
        "name" : "Sample Project",
        "id" : "sample-project",
        "description" : "An example Project",
    }

**Response:**

    HTTP/1.1 201 OK

# Working with Project Iterations

## Retrieving a Project Iteration

    GET /projects/p/{project_id}/iterations/i/{iteration_id}
    Host: www.example.com
    Accept: application/json
**Response:**
    HTTP/1.1 200 OK
    Content-Type: application/json
    Content-Length: 1445
    
    {
         "name":"Fedora 13",
         "id":"f13",
         "revision":1,
         "summary":"Fedora 13",
         "documents":[{"name":"Managing_software","id":"Managing_software",
                       "path":"","content-type":"application/x-gettext",
                       "revision":1,"lang":"en-US","links":[]},
                      {"name":"Media","id":"Media",
                       "path":"","content-type":"application/x-gettext",
                       "revision":1,"lang":"en-US","links":[]}],
         "links":[]
    }

## Create/Update a Project Iteration

    PUT /projects/p/{project}/iterations/i/{iteration}
