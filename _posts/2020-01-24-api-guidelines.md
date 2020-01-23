---
layout: post
title: "Thoughts on API Guidelines"
date: 2020-01-24 12:00:00
published: true
categories: posts
tags:  api guideline rest http http2 swagger openapi
---


## Introduction

Within a few large Enterprises that I have had the chance to work at, below are my general thoughts and heavily swagger-influenced take on a good API.

This post will change over time. Last updated 01/24/20.

## Overview

APIs are sets of requirements that govern how one application can communicate and interact with another. APIs may be used by both developers inside the organization that published the API or by any developers outside that organization who wish to register for access to the interface.

Some of the keys to a successful API are an **Interface-first design, discoverability, reuse, engagement, and monitoring**.

The quickest way to get running with a successful API is to understand the OpenAPI Specification - also known as the Swagger Specification. It is a specification for machine-readable interface files for describing, producing, consuming, and visualizing RESTful web services. The OpenAPI specification along with related tools can generate code for both clients and servers, documentation and test cases given an interface or 'specification' file.

## REST

**Re**presentational **s**tate **t**ransfer

REST is a style, not a standard
Like other web services, REST uses HTTP as the transport protocol for the message requests and responses. However, unlike other web services (e.g. SOAP), REST is an architectural style, not a standard protocol. This is why REST APIs are sometimes called RESTful APIs — REST is a general style that the API follows.

A RESTful API might not follow all of the official characteristics of REST as outlined by Dr. Roy Fielding, who first described the model. Hence these APIs are “RESTful” or “REST-like.” (Some developers insist on using the term “RESTful” when the API doesn’t fulfill all the characteristics of REST, but most people just refer to them as “REST APIs” regardless.)

### REST Architectural Constraints

#### Client-server

The client application and server application must be able to evolve separately without any dependency on each other. A client should know only resource URIs and that’s all.

#### Stateless

Inspired from HTTP - make all client-server interaction stateless. Server will not store anything about latest HTTP request client made. It will treat each and every request as new. No session, no history. Though depending on the security scheme chosen some caveats to state will be accepted. Client is responsible for state, each request from the client should contain all the information necessary to service the request – including authentication and authorization details.

#### Cacheable

Caching brings performance improvement for client side, and better scope for scalability for a server because the load has reduced. Caching shall be applied to resources when applicable and then these resources declare themselves cacheable. Caching can be implemented on the server or client side.

#### Layered system

REST allows you to use a layered system architecture where you deploy the APIs on server A, and store data on server B and authenticate requests in Server C, for example. A client cannot ordinarily tell whether it is connected directly to the end server, or to an intermediary along the way.

#### Uniform interface

As the constraint name itself applies, you must decide APIs interface for resources inside the system which are exposed to API consumers and follow religiously. A resource in the system should have only one logical URI and that should provide a way to fetch related or additional data. This leads to the emphasis on interface-first design principles.

#### Code on demand (optional)

Most of the time you will be sending the static representations of resources in form of XML or JSON. Optionally you are free to return executable code to support a part of your application e.g. to get a UI widget rendering code.

#### REST Architectural Elements

| Data Element        | Modern Web Examples |
| ------------- |-------------|
| resource     | the intended conceptual target of a hypertext reference |
| resource identifier| URL, URN |
| representation | HTML document, JPEG image |
| representation metadata | media type, last-modified time |
| resource metadata | source link, alternates |
| control data | if-modified-since, cache-control |

#### RESTful Web Service

Another unique aspect of REST is that REST APIs focus on **resources** (that is, *things*, rather than actions) and ways to access the resources. Resources are typically different types of information. You access the resources through URLs (Uniform Resource Locators), just like going to a URL in your browser retrieves an information resource. The URLs are accompanied by a method that specifies how you want to interact with the resource.

#### URL and HTTP methods

Resource Collection

`http://api.example.com/resources/`

| Action        | Results |
| ------------- |-------------|
| GET | List of collection's members |
| PUT | Replace the entire collection with another collection. |
| POST | Create a new entry in the collection. |
| DELETE | Delete the entire collection. |

Resource Element

`http://api.example.com/resources/17`

| Action        | Results |
| ------------- |-------------|
| GET | Retrieve a representation of the addressed member of the collection.|
| PUT | Replace the addressed member of the collection, or if it does not exist, create it. |
| POST | Not generally used. Treat the addressed member as a collection in its own right and create a new entry within it. |
| DELETE | Delete the addressed member of the collection |

## OpenAPI

[OpenAPI](https://www.openapis.org/about) is API description language which is focusing on creating, evolving and promoting vendor neutral description format

You can write your API spec with OpenAPI

The OpenAPI Specification (OAS) defines a standard, language-agnostic interface to RESTful APIs which allows both humans and computers to discover and understand the capabilities of the service without access to source code, documentation, or through network traffic inspection. When properly defined, a consumer can understand and interact with the remote service with a minimal amount of implementation logic.

An OpenAPI definition can then be used by documentation generation tools to display the API, code generation tools to generate servers and clients in various programming languages, testing tools, and many other use cases.

What is OpenAPI

- Supported spec format
- YAML and JSON
- Based on JSON schema
- Vocabulary to annotate and validate JSON
- Originally known as Swagger

How to use OpenAPI

As API documents

Basic Structure

You can write OpenAPI definitions in YAML or JSON. A sample OpenAPI 3.0 definition written in YAML looks like:

```yaml
openapi: 3.0.0
info:
  title: Sample API
  description: Optional multiline or single-line description in [CommonMark](http://commonmark.org/help/) or HTML.
  version: 0.1.9

servers:
  - url: http://api.example.com/v1
    description: Optional server description, e.g. Main (production) server
  - url: http://staging-api.example.com
    description: Optional server description, e.g. Internal staging server for testing

paths:
  /users:
    get:
      summary: Returns a list of users.
      description: Optional extended description in CommonMark or HTML.
      responses:
        '200':    # status code
          description: A JSON array of user names
          content:
            application/json:
              schema:
                type: array
                items:
                  type: string
```

The key components of the specification are: [Metadata](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.0.md#info-object), [Servers](https://swagger.io/docs/specification/api-host-and-base-path/), [Paths](https://swagger.io/docs/specification/paths-and-operations/), [Parameters](https://swagger.io/docs/specification/describing-parameters/), [Request Body](https://swagger.io/docs/specification/describing-request-body/), [Responses](https://swagger.io/docs/specification/describing-responses/), Input/Output Models, and [Authentication](https://swagger.io/docs/specification/authentication/). The full OpenAPI 3.0 Specification is available on [GitHub](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.0.md)

With this specification you are then able to generate good looking documents, share API spec and iterate, develop independently frontend and backend or client and server, understandable with both developers and non-developers.

The Swagger organization in conjunction with the OpenAPI Specification has created API tools, these perform

- Code generation
- Code for request data validation in server
- Code for API calling in clients
- Visualize and Testing API documents

As a quick background, some of the OpenAPI Competitors:

- RESTful API DL (Description Language)
- RAML (RESTful API Modeling Language)
- YAML base
- API Blueprint
- Markdown base

## OpenAPI Tools

The tools that make the OpenAPI Specification powerful are:

- [Swagger Editor](https://swagger.io/tools/swagger-editor/)
- [Swagger Codegen](https://swagger.io/tools/swagger-codegen/)
- [Swagger UI](https://swagger.io/tools/swagger-ui/)

### Swagger Editor

Design, describe, and document your API on a open source editor fully dedicated to OpenAPI-based APIs. The Swagger Editor is great for quickly getting started with the OpenAPI specification, with support for Swagger 2.0 and OpenAPI 3.0.

It is a WYSIWYG Spec editor in web browser that provides syntax highlighting, autocompletion, and real time spec validation

### CodeGen

Swagger Codegen can simplify your build process by generating server stubs and client SDKs for any API, defined with the OpenAPI specification, helping everyone focus on better API implementation and adoption.

Generating server stubs and client SDKs for any API.

### Swagger UI

Swagger UI allows anyone, developers or consumers, to visualize and interact with the API’s resources without having any of the implementation logic in place. It’s automatically generated from the OpenAPI Specification, with the visual documentation making it easy for back end implementation and client side consumption.

## OpenAPI Security

A security scheme definition is a global definition with a name that designates an authentication method available for the API. It is possible to define multiple schemes if, for example, an API supports both Basic Authentication and OAuth. There can even be various schemes of the same type.

A security requirement, on the other hand, can exist both globally for the entire API or on an operation level. It matches the API as a whole or the individual API operation to one or more of the previously defined security schemes. Developers can use this to specify different levels of authorization explicitly, for example, by making read requests available without authentication, but requiring OAuth for write requests.

OpenAPI lets you describe APIs protected using the following security schemes:

- HTTP authentication schemes (they use the Authorization header):
  - [Basic](https://swagger.io/docs/specification/authentication/basic-authentication/)
  - [Bearer](https://swagger.io/docs/specification/authentication/bearer-authentication/)
  - other HTTP schemes as defined by [RFC 7235](https://tools.ietf.org/html/rfc7235) and H[TTP Authentication Scheme Registry](https://www.iana.org/assignments/http-authschemes/http-authschemes.xhtml)
- [API keys](https://swagger.io/docs/specification/authentication/api-keys/) in headers, query string or cookies
  - [Cookie authentication](https://swagger.io/docs/specification/authentication/cookie-authentication/)
- [OAuth 2](https://swagger.io/docs/specification/authentication/oauth2/)
- [OpenID Connect Discovery](https://swagger.io/docs/specification/authentication/openid-connect-discovery/)

This example shows how various security schemes are defined. The *BasicAuth*, *BearerAuth* names and others are arbitrary names that will be used to refer to these definitions from other places in the spec.

```yaml
components:
  securitySchemes:

    BasicAuth:
      type: http
      scheme: basic

    BearerAuth:
      type: http
      scheme: bearer

    ApiKeyAuth:
      type: apiKey
      in: header
      name: X-API-Key

    OpenID:
      type: openIdConnect
      openIdConnectUrl: https://example.com/.well-known/openid-configuration

    OAuth2:
      type: oauth2
      flows:
        authorizationCode:
          authorizationUrl: https://example.com/oauth/authorize
          tokenUrl: https://example.com/oauth/token
          scopes:
            read: Grants read access
            write: Grants write access
            admin: Grants access to admin operations
```

## Asynchronous

[Callbacks](https://swagger.io/docs/specification/callbacks/)

In OpenAPI specs, you can define callbacks – asynchronous, out-of-band requests that your service will send to some other service in response to certain events. This helps you improve the workflow your API offers to clients. A typical example of a callback is a subscription functionality – users subscribe to certain events of your service and receive notification when this or that event occurs.

Walk through the spec:

callbacks are defined inside the related operation - post, put, and so on (not under the path itself). In this example, under the post method of the /subscribe path:

```yaml
paths:
  /subscribe:
    post:
      …
      callbacks:
        …
```

## API Management

API Management with an **API Gateway** is not required for a great API design, however they are helpful in platform implementation.

An API gateway is the single entry point for all clients. The API gateway handles requests in one of two ways. Some requests are simply proxied/routed to the appropriate service. It handles other requests by fanning out to multiple services. Rather than provide a one-size-fits-all style API, the API gateway can expose a different API for each client. Most of the times, the API gateway might also implement security, e.g. verify that the client is authorized to perform the request.

The API Gateway provides an extra layer in front of your API and can be managed by 3rd parties (externalized).

Features:

- Authentication, Authorization
- Role-based security
- DDOS attack protection
- Monetization: pay per use
- Scaling
- Auditing
- Usage metrics, analytics

Examples technology providers:

- 3scale
- Apigee
- Mashape Kong
- CA 7Layers
- AWS API Gateway
- Azure API Management
- IBM Bluemix API Management
- WSO2

These products typically take the OpenAPI Specification as the input to their platforms, being able to immediately consume and understand your backend.

API Versioning can be offloaded to the API Gateway, examples:

- Versioning via URL
  - GET */v1/restaurants?location=SVQ*
  - GET */v2/restaurants?location=SVQ&limit=30*
- Versioning via parameter
  - GET */restaurants?location=SVQ&limit=30&v=2*
- Versioning via headers
  - GET */restaurants?location=SVQ&limit=30*
  - Header key: *Version - Header value: 2*

API Scalability can also be offloaded to the API Gateway:

- Stateless API
- Load-balancer to handle traffic (e.g. nginx or ha-proxy)
- Distributes traffic to your API servers
- DNS, SSL and certs. configured in the load balancer

## Beyond HTTP/1.1

As technology evolves and new standards are implemented, some of the principles of a successful API remain: **Interface-first design, discoverability, reuse, engagement, and monitoring**.

One technology stack rising today is [gRPC](https://grpc.io/about/). gRPC, is an RPC framework based on protobufs and HTTP/2, that can make building and consuming APIs easier and more performant. As many clients speak http/1.1 plus json, the interoperability with JSON and Open API is very important. For users who are more comfortable with HTTP/1.1+JSON-based APIs and the Open API Initiative Specs APIs there are a combination of open source libraries to make gRPC services available in that form as well. There are a few groups that have built API multiplexers to give users the best of both worlds.

From CoreOS is an example of a [gRPC + REST Gateway](https://github.com/philips/grpc-gateway-example)

Another great example is from the NYTimes with their [openapi2proto](https://github.com/NYTimes/openapi2proto).
