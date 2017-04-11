---
title: Marketing Suite API Reference

language_tabs:
  - json
  - xml

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

1. <a href="?http#api-category-data-management">Data Management</a>
2. <a href="Campaign Management
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

* For users in the U.S.: <a href="https://api.eccmp.com/services2/help/">https://api.eccmp.com/services2/help/</a>
* For users in Europe: <a href="https://api.ccmp.eu/services2/help/">https://api.ccmp.eu/services2/help/</a>

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
XML Support |   | Y | Y | Y | Y
JSON Support |   | Y | Y |   | Y
OAuth 2.0 authentication |   | Y | Y | Y | Y
Load data to joined tables | Y | Y | Y | Y | Y
Multiple records in the same request |   |   | Y | Y |  
Endpoint can be used to trigger a Campaign | Y | Y | Y | Y |  

## HTTP POST

The HTTP POST endpoint is used to submit data collected from your consumers via a Web Form. The Web Form can either be hosted on the Marketing Suite platform, or hosted externally on some other site. Only one record can be submitted per API request. The input parameters and their associated values are all contained within the header of the HTTP request, rather than in the body.

This endpoint does not support authentication, nor does it support JSON or XML. The request can optionally be secured by using the HTTPS protocol, as long as the correct Customer ID and Form ID are provided, and the right field names are posted to the Form. If using HTTPS, please note that the connection is encrypted, but the data being passed is not encrypted.

The following diagram depicts the processing flow for loading data via the HTTP POST endpoint.

# Authentication

"Authentication" refers to the process of verifying that the person requesting a service is, in fact, who he says he is. Within Marketing Suite, authentication is handled by an open standard protocol called Oauth 2.0. This protocol was designed specifically for HTTP, and provides standard mechanisms to allow REST API users to request access to a particular service.

Authentication with Oauth 2.0 consists of several steps. In the first step, the end-user obtains a "Consumer Key" and a "Consumer Secret." The Consumer Key is analogous to a username, and is considered public information; the Consumer Secret is analogous to a password, and is kept confidential. Both of these pieces are managed at the user level, and can be obtained from within the Marketing Suite user interface (see the Generating Your Consumer Key and Consumer Secret section below for details on this process).

Oauth 2.0 supports a handful of different methods called "grant types" for granting access to a requested service. Marketing Suite utilizes only one grant type: "Password." The second step in the authentication process involves using this grant type to request a "token." A token is a text string that, when provided in a request message, will allow the user access to the requested service. Tokens are valid only for a certain period of time. By default, Marketing Suite tokens expire after eight hours, but you can optionally adjust this duration (see the Modifying Your Security Settings section for details on this process).

The Oauth 2.0 protocol defines several different types of tokens. Marketing Suite uses the most common type of token, known as "bearer." A bearer token is a randomly-generated text string without any sort of encryption key. When you use this token to make an API service call, you are assumed to be the owner, or "bearer" of the token.

To get a token, you must provide your credentials (the Consumer Key and Secret) directly to the authentication server via a POST request (see the Requesting Your Token section below for details on his process). If these credentials are valid, the server replies back with the token. With this token now in hand, you are now fully authenticated, and you can begin making REST API service calls.

The following diagram depicts this authentication process.

<img src="images/diagram1.png"/>

## Generating Your Consumer Key and Consumer Secret

The Consumer Key and Consumer Secret are used to request a token. These credentials are managed at the user level. You can view, set, and edit your API credentials from the Security Setting screen in Marketing Suite.
To generate your Consumer Key and Consumer Secret:

1. In the System Tray, select Data Integration > Settings > API Keys. The **API KEYS** screen is displayed.
2. Within the "Oauth 2 API Keys" section, you'll find your Client ID, Consumer Key, and Consumer Secret.

If you ever need to reset or update your Consumer Secret, navigate to the API KEYS screen as described above, then click "Regenerate Secret." The system generates a new Consumer Secret, and displays it on the screen.

## Requesting Your Token

> Below is a sample token request message:

```http
POST https://api.eccmp.com/services2/authorization/oAuth2/Token HTTP/1.1
Host: api.eccmp.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 98
username=NTcwNjozOTQ=&password=1c106f90ec274340bde50ea78f410422&client_id=5706&grant_type=password
```
> Below is a sample token response message (for the sake of readability, the token depicted here is much shorter than what a real token would be):

```json
{
"access_token":"AAEAAG39ZdZRoGDRZJggMdv43pxrIVokFD57mhz03ncF",
"token_type":"bearer",
"expires_in":28800,
"refresh_token":"h8fR!IAAAAFMbAP2AWWeil7JS9YKx3mURSZypddIawaUJQpBjUYarGI2g"
}
```

Once you have your Consumer Key and Consumer Secret, the next step in the authorization process is to submit a request message to obtain a token from the Oauth server. This message must be submitted using the POST method, with a Request Content Type of <application/x-www-form-urlencoded>.

The request message must contain the following parameters:

Keyword | Required | Description
-|-|-
username | Required | Your Consumer Key, found on the **API ACCESS** screen
password | Required | Your Consumer Secret, found on the **API ACCESS** screen
client_id | Optional | If used, must be set to your client identifier, which can be found on either the **EDIT USER ACCESS RIGHTS** screen or the **API ACCESS** screen.
grant_type | Required | "password" (no quotes)

To request a token:

1. Submit the request message using the POST method, with the parameters defined above, to the following URL:
  * https://api.eccmp.com/services2/authorization/oAuth2/Token
2. If your call fails, you'll receive an error response message, indicating that the token has not been generated. If your call is successful, you'll receive a response message in JSON format containing the following information:

Keyword | Required
-|-
access_token | The Oauth 2.0 token.
token_type | This value will always be “bearer."
expires_in | Lifetime of the token in seconds.
refresh_token | This parameter is not currently used.

### Using Your Token

With your valid token, you can now begin making REST API service calls, utilizing any of the platform's RESTful APIs. The token must be provided within the header of each of your web service calls. Each call must contain an authorization to Bearer type, followed by your token text string.

Below is an example of an HTTP header:

`Authorization: Bearer 123456789761657894564564fh7rgjuhsrn56yu4567y56re4qnu56sr`

As noted above, your token will expire after a set period of time. If you attempt an API request call, and your token has expired, you'll receive a response message with the "401 - Unauthorized" response code. At this point, you'll need to repeat the steps described above to request a new token. Once you've received this new token, you can begin making API request calls again.

# Payload

```json
// The three main sections in a JSON payload look like this: (General structure of the AET payload)

{
"_data": { … },
"_campaignMetadata": { … },
"_importOptions":
}
```

```xml
<!-- The three main sections in a XML payload look like this: (General structure of the AET payload) -->

<_apiData xmlns:json="http://james.newtonking.com/projects/json">
  <_data>
  </_data>
  <_campaignMetadata>
  </_campaignMetadata>
  <_importOptions>
  </_importOptions>
</_apiData>
```

The Advanced Event Trigger request payload is composed of the following three main objects:

* **_data** (mandatory): This section defines the entity relationships for the payload, and contains the data for the relational insert and the campaign message.
* **_campaignMetadata** (optional): If used, this section contains campaign data that is never inserted into an entity, but is available in a campaign for content blocks (not available for use in looping blocks).
* **_importOptions** (optional): If used, this section defines the import rules for each entity present in the payload. If no section is present for a given entity, the data will be imported according to the defaults behavior for those fields.

## Section: _data

```json
//Simple _data section with one parent join and one child join for JSON payload

{
  "_data": {
    "recipient": [
      {
        "name_first": "John",
        "name_last": "Smith",
        "email": "john.smith@gmail.com",
        "recipient_to_organization": { //An "upward" join - to a parent
          "organization_name": "Experian",
          "organization_city": "Costa Mesa"
        },
        "order.order_to_recipient": [ // A "downward" join - to a child
          {
            "order_no": "2834737",
            "total": "129.99",
          },
          {
            "order_no": "2834738",
            "total": "59.99",
          }
        ]
      }
    ]
  }
}
```

```xml
<!-- Simple _data section with one parent join and one child join for XML payload -->

<_apiData xmlns:json="http://james.newtonking.com/projects/json">
  <_data>
    <recipient json:Array="true">
      <name_first>John</name_first>
      <last_name>Smith</last_name>
      <email>john.smith@gmail.com</email>
      <recipient_to_organization json:Array="true"> <!-- An "upward" join - to a parent -->
        <organization_name>Experian</organization_name>
        <organization_city>Costa Mesa</organization_city>
      </recipient_to_organization>
      <order.order_to_recipient json:Array="true"> <!-- A "downward" join - to a child -->
        <order_no>2834737</order_no>
        <total>129.99</total>
        </order.order_to_recipient>
          www.experian.com/marketingservices Advanced Event Trigger Technical Guide | Version 1.5 | Page 14
        <order.order_to_recipient json:Array="true">
        <order_no>2834738</order_no>
        <total>59.99</total>
      </order.order_to_recipient>
    </recipient>
  </_data>
</_apiData>
```

The _data object is a nested series of JSON objects, each one representing an entity record.

If you are sending data to the AET endpoint to trigger a campaign send, the root node within _data must be the target entity for the campaign you wish to send the message. For instance, if the campaign you wish to trigger was created targeting the entity named “recipient,” then the root node of _data must be “recipient.”

In the figure below, you can see the structure for the _data section. First, notice that the root node within _data is named “recipient,” and that it's a collection of objects. At the current time, relational insert and AET don't support multiple records in this root node collection (i.e., sending bulk or multiple messages at once, or inserting multiple records at once, for the target entity).

## Section: _campaignMetadata

The payload can contain an additional section called _campaignMetadata that is never inserted into the platform’s entity tables, but contains additional information available for personalization within the campaign or in content blocks (not supported on the entities that require looping blocks to personalize). The objects in this section are explicitly never saved, as they don’t relate to physical columns or the relational structure.

This section is optional, and it’s irrelevant when using the Relational Data Insert endpoint.