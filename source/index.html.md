---
title: Marketing Suite API Reference

language_tabs:
  - javascript

toc_footers:
  - <a href='https://api.eccmp.com/services2/help/'>Swagger UI</a>

includes:

search: true
---

# Introduction

## Purpose

The purpose of this document is provide a high-level overview of the Application Programming Interface (API) capabilities offered by Experian's Marketing Suite platform.
APIs allow clients to simplify and / or automate the process of communicating with Marketing Suite for a wide range of processing functions. For example, you can use an API to load data into your database, to trigger the deployment of a marketing Campaign, or to create a new Filter.

The key benefit to an API is that the information passed back and forth is structured and pre-defined, so there's no ambiguity. The expectations on both sides are clearly defined. In simplest terms, an API provides a common understanding between two systems that states, "If you provide me with these instructions, I will perform the specified action, or return the requested information."

## API Categories

The Marketing Suite API endpoints are grouped into the following categories, all of which are described below in more detail.

1. Data Management
2. Campaign Management
3. Campaign Deployment
4. Reporting

## Architecture

Through the use of different commands called "HTTP methods," many of the Marketing Suite endpoints provide a full range of functionality -- creating new items, updating or deleting existing items, as well as requesting information about a specific item.

* GET: Used to read or retrieve data without directly modifying it.
* POST: Used to create a new asset.
* PUT: Used to update an existing asset.
* PATCH: Used to modify an existing asset; the request needs to contain only the necessary changes to the asset, not the complete asset.
* DELETE: Used to delete an existing asset.

Please note that not every method is supported by every endpoint. For example, the "Email Campaign" endpoint allows you to use GET, POST, PUT, and DELETE, but the "Preview" endpoint supports only the POST method.

Most of the Marketing Suite API endpoints are built using REST, or "REpresentational State Transfer.” REST is in architectural style that imposes a set of constraints on all components in the system. These constraints control the way that components interact with one another. A service built using these constraints is said to be "RESTful."

Unlike other architectures, REST allows you to specify the data you want to access via a URL (with the ability to use optional parameters), and to specify the action to be taken via the HTTP method. Responses are returned in the expected format, along with a standard HTTP return code.

### Authentication

Most Marketing Suite API endpoints require security authentication. "Authentication" refers to the process of determining that you are, in fact, who you say you are.

Within Marketing Suite, authentication is handled by an open standard protocol called OAuth 2.0. This protocol was designed specifically for HTTP, and provides standard mechanisms to allow REST API users to request access to a particular service.

For more information on authentication, including how to request your OAuth 2.0 token, please see the the Marketing Suite API How-to Guide or the Marketing Suite Online Help system.

### Authorization

The Authentication process described above is designed to validate that "you are who you say you are." The second security check utilized by Marketing Suite is Authorization, which validates that you have the proper access privileges to the service that you requested.

Authorization for the Marketing Suite APIs is handled through the same roles, groups, and privileges that are used in the platform's user interface. As an example, if your user account is configured to allow you to create Campaigns in the Marketing Suite interface, then you can also use the Campaign endpoint to create Campaigns.

If you attempt an API request for a service for which you have not been granted access, you will receive either a "401 - Unauthorized" error message, or a "403 - Forbidden" error message.

You can find details regarding your user account and privileges on the Security Settings screen within the Marketing Suite application.

## Additional Resources

Additional information about most of the Marketing Suite API endpoints can be found on our developer portal:

* For users in the U.S.: https://api.eccmp.com/services2/help/
* For users in Europe: https://api.ccmp.eu/services2/help/

# API Category: Data Management

## Overview
Marketing Suite allows you to load data into your database by means of API calls. The platform provides several different endpoints, with different features designed to meet your specific requirements.

The Data Management endpoints are as follows:

* HTTP POST
* Standard Data Load
* Batch Import
* Sequential Data Load
* Advanced Data Load

The following table summarizes the differences between the different Data Management endpoints:

   | HTTP POST | Standard Data Load | Batch Import | Sequential Data Load | Advanced Data Load
---|-----------|--------------------|--------------|----------------------|--------------------
XML Support | X | Y | Y | Y | Y
JSON Support | X | Y | Y | X | Y
OAuth 2.0 authentication | X | Y | Y | Y | Y
Load data to joined tables | Y | Y | Y | Y | Y
Multiple records in the same request | X | X | Y | Y | X
Endpoint can be used to trigger a Campaign | Y | Y | Y | Y | X

## HTTP POST

The HTTP POST endpoint is used to submit data collected from your consumers via a Web Form. The Web Form can either be hosted on the Marketing Suite platform, or hosted externally on some other site. Only one record can be submitted per API request. The input parameters and their associated values are all contained within the header of the HTTP request, rather than in the body.

This endpoint does not support authentication, nor does it support JSON or XML. The request can optionally be secured by using the HTTPS protocol, as long as the correct Customer ID and Form ID are provided, and the right field names are posted to the Form. If using HTTPS, please note that the connection is encrypted, but the data being passed is not encrypted.

The following diagram depicts the processing flow for loading data via the HTTP POST endpoint.