# Overview of Flies Rest-like API (1.0)

# Base URL

The Base URL for all REST resources in this API is `/seam/resource/restv1/`.

# Authentication

REST requests are authenticated using `X-Auth-User` and `X-Auth-Token` headers. These correspond to the Flies user name and API Key. Within the Flies web interface, the API Key can be retrieved and re-generated from the *My Profile* page.

**Example:**

    GET /projects HTTP/1.1
    Host: www.example.com
    Accept: application/json
    X-Auth-User: demo
    X-Auth-Token: 01234567890123345

Accessing resources and operations that require authentication with invalid or missing credential will result in a `401 Unauthorized` status code.

Accessing resources and operations for which valid credentials have been provided - but for which the authenticated user is not authorized to access - will also result in a `401 Unauthorized` status code.

# Media Types

TODO

# Rest Services

- [Projects](RestProjectsV1)