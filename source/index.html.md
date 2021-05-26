---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

includes:

search: true

code_clipboard: true
---

# Introduction
Invenias has a REST API for integrations, providing programmatic access to create, read, update and delete data from your Invenias database.

The Invenias API Swagger UI documentation can be found by navigating to https://{subdomain}.invenias.com/api, where you need to enter your subdomain to this URL.

## General Conventions
Whenever possible, the REST API will follow specifications, conventions, and practices of HTTP and the web in general. This includes things like case-sensitivity of URLs, character encodings, HTTP methods, and so forth. Additional recommended practices for REST APIs are also observed whenever appropriate.

### Entities
Invenias uses the term entity to refer to a type represented in the Invenias system. People, Companies, Assignments, and Placements are examples of entities. Entities capture the core concepts within the Invenias system and provide an organization for storing executive search data and applying the rules and processing that comprise the Invenias system.

### JSON 
The REST API follows the specifications and conventions of the JavaScript Object Notation (JSON) data format and any related Javascript syntax specifications. For more information, see the following articles:
<ul>
<li>[http://www.json.org](http://www.json.org)</li>
<li>[http://en.wikipedia.org/wiki/JSON](http://en.wikipedia.org/wiki/JSON)</li>
</ul>

# Creating an API Application
## POST /api/v1/thirdpartyapplications
```shell
# Example Response:
{
  "ClientId": "your_client_id",
  "ClientSecret": "your_client_secret",
  "ExpiresOn": "2022-05-12T15:52:47.2298517+00:00",
  "IsEnabled": true,
  "Name": "Documentation Test",
  "ReplyUrl": "https://example.com",
  "FlowType": "ResourceOwner"
}
```
> Please note, this is the only time you'll get the client_id and client_secret - please store these securely.</aside>

To create a new integration, use the POST /api/v1/thirdpartyapplications endpoint using Swagger. 

<aside class="notice">
Please note, you must have a licensed Invenias User Account and be in the 'System Administrator' permission group to be able to perform this operation.
</aside>

Before you can use the POST /api/v1/thirdpartyapplications endpoint, you need to enter an `api_key` in the field at the top right-hand corner of the Swagger page. To do this simply double click into the `api_key` field in the header to automatically generate one. This will prompt you to log in if you're not already logged in. 

An `api_key` will be generated which will be valid for 3 minutes before expiring. You can clear this field and double click it again to generate a new one to gain access for another 3 minutes.

After Swagger returns you an `api_key`, you can then call the POST /api/v1/thirdpartyapplications endpoint to generate the `client_id` and the `client_secret`. 

From here, you will need to choose the flow type that your application requires. There's some documentation available [here](https://auth0.com/docs/flows) to help.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/thirdpartyapplications`

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | How long the Application is valid for: `OneYear`, `TwoYears`, or `FiveYears`.
Name | [required] | The friendly name of your Application.
FlowType | `ResourceOwner` | The OAuth 2,0 Authorization flow: This can be either `ResourceOwner` or `Code`.
ReplyURL (Optional)| | The post-login URL to redirect to your Application.

# Renewing an Application
## POST /api/v1/thirdpartyapplications/{id}/renew
```shell
# Example Response:
{
  "ClientId": "your_client_id",
  "ClientSecret": "your_client_secret",
  "ExpiresOn": "2026-05-18T13:34:16.9289531+00:00",
  "IsEnabled": true,
  "Name": "Documentation Test",
  "ReplyUrl": "https://example.com",
  "FlowType": "ResourceOwner"
}
```
> Please note, this is the only time you'll get the client_secret - please store it securely.</aside>

To renew an expired Application use the POST /api/v1/thirdpartyapplications/{id}/renew endpoint using Swagger. 

<aside class="notice">
Please note, you must have a licensed Invenias User Account and be in the 'System Administrator' permission group to be able to perform this operation.
</aside>

Before you can use the POST /api/v1/thirdpartyapplications/{id}/renew endpoint, you need to enter an `api_key` in the field at the top right-hand corner of the Swagger page. To do this simply double click into the `api_key` field in the header to automatically generate one. This will prompt you to log in if you're not already logged in. 

An `api_key` will be generated which will be valid for 3 minutes before expiring. You can clear this field and double click it again to generate a new one to gain access for another 3 minutes.

After Swagger returns you an 'api_key', you can then call the POST /api/v1/thirdpartyapplications/{id}/renew endpoint to renew the expired Application.

<aside class="warning">
Please note, when renewing your Application it will be issued a new client_secret, please remember you update this in your Application or you will not be able to successfully authenticate.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/thirdpartyapplications/{client_id}/renew?expiration=FiveYears`

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | This is the unique identifier for the expired Application you wish to renew. 
expiration | [required] | The period of time you wish the renew the Application for: `OneYear`, `TwoYears`, or `FiveYears`.

# Authentication
## POST /identity/connect/token

> To authorize, use this code:

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/identity/connect/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'subdomain={subdomain}' \
--data-urlencode 'password={password}' \
--data-urlencode 'client_secret={client_secret}' \
--data-urlencode 'grant_type=password' \
--data-urlencode 'client_id={client_id}' \
--data-urlencode 'scope=openid profile api email' \
--data-urlencode 'username={username}'

# Example Response:
{
    "access_token": "{token}",
    "expires_in": 86400,
    "token_type": "Bearer"
}
```

The Invenias API uses the OAuth 2.0 standard for authentication.

Every API integration has a `client_id` and `client_secret`, which are used when authenticating along with valid user credentials. The Invenias API supports the following grant types for authenticating:

<ul>
<li>Password (ResourceOwner)</li>
<li>Authorization Code (Code)</li>
<li>Refresh Token</li>
</ul>

Integrations using the Password grant type require a username and password to be posted as part of the authentication request instead of showing the Invenias login screen. As the user’s credentials are password directly to Invenias without going via the login screen, only a user account using the Invenias Identity Provider can be used to authenticate.

Here’s an example of authenticating when using the password grant type:

### HTTP Request
`https://{subdomain}.invenias.com/identity/connect/token`

Parameter | Default | Description
--------- | ------- | -----------
username | [required] | Invenias user account username.
password | [required] | Invenias user account password.
client_id | [required] | This is the unique identifier for your Application.
client_secret | [required] | This is a secret key known only to the application and the authorization server.
grant_type | password | The method an application uses gets an access token.
scope | openid profile api email | Scope is a mechanism in OAuth 2.0 to limit an application's access to a user's account.

You will need to use this token to authenticate when using all endpoints using the Authorization header using the format "Authorization: {type} {credentials}", where the type is your `token_type` and the credentials are your `access_token`.

Integrations using the Authorization Code grant type use a redirect URL to direct users to the Invenias login screen as part of the authentication process, requiring initial user interaction to trigger the integration. As part of Invenias X, one of the new features is Single Sign-On, as we allow customers to configure multiple Identity Providers for their users. Users can authenticate with the API using any of the enabled Identity Providers.

<aside class="notice">
The token is valid for 24 hours. We use short-lived access tokens for security and this period cannot be extended.

The tokens are issued with a short expiration time so that the application is forced to continually refresh them, giving the service a chance to revoke an application’s access if needed.

When the access token expires, the application can use the refresh token to obtain a new access token. It can do this behind the scenes, and without the user’s involvement so that it’s a seamless process to the user. However, this will require additional code to perform this function.
</aside>

### Unauthorized requests
If a client request contains an invalid `bearer_token`, the server returns response status 401 (unauthorized). For more information on HTTP status codes see [here](https://bullhorn.github.io/invenias-api-docs/#http-response-status-codes).

### Identifying if an OAuth token has expired

There are two common approaches adopted by web developers to identify when an OAuth token has expired:

<b> Token Refresh Handling: Method 1 </b>

Upon receiving a valid `access_token`, `expires_in value`, `refresh_token`, etc., clients can process this by storing an expiration time as a local variable and checking it on each request. This can be done using the following steps:

<ol>
	<li>Convert expires_in to an expire time (epoch, RFC-3339/ISO-8601 datetime, etc.)</li>
	<li>Store the 'expire time'.</li>
	<li>On each resource request, check the current time against the 'expire time' and make a token refresh request before the resource request if the token has expired.</li>
</ol>

When checking the time, be sure you are at the same time, for example, using the same timezone by converting all times to epoch or UTC timezone.

In addition to receiving a new `access_token`, you may receive a new `refresh_token` with an expiration time further in the future. If you receive this, you should store the new refresh_token to extend the life of your session.

<b>Token Refresh Handling: Method 2</b>

Another method of handling token refresh is to manually refresh after receiving an invalid token error. This can be done with the previous approach or by itself.

If you attempt to use an expired access_token and you get an invalid token error, you should perform a token refresh. Since different services can use different error codes for expired tokens, you can either keep track of the code for each service, or an easy way to refresh tokens across services is to simply try a single refresh upon encountering a 401 error. 

# Rate Limiting
We use a fixed-window rate limiting strategy, up to `3000` api calls can be made at any interval within a 5 minute window.

You can find out how many requests you have remaining by checking the value in the `X-Request-Quota-Remaining reaches` header.

<img src="images\quotaremaining.png" alt="X-Request-Quota-Remaining" class="inline"/>
<i>Figure 1. API Call Response Headers - X-Request-Quota-Remaining</i>

Once the `X-Request-Quota-Remaining reaches` 0, all succeeding requests made will return `429` errors, until the current rate limit window resets. At this time your Application should wait for the limit to become available again.

<aside class="warning">
Failure to add adequate mechanisms to your Application instructing it to wait after receiving a 429 error may result in the user account used to authenticate the requests being disabled by our BTO team. This would happen should it generate 9000 (or more) 429 errors within any given 5-minute period. The account will remain disabled until their satisfied that steps have been taken to prevent the problem from occurring again.
</aside>

# HTTP response status codes
HTTP response status codes indicate whether a specific HTTP request has been successfully completed.
## Successful Responses

Code | Name | Description
---- | ---- | -----------
200 | OK | The request has succeeded.
201 | Created | The request has succeeded and a new resource has been created as a result.
204 | No Content | There is no content to send for this request, but the headers may be useful.

## Redirection Messages
Code | Name | Description
---- | ---- | -----------
301 | Moved Permanently | The URL of the requested resource has been changed permanently. The new URL is given in the response.
302 | Found | This response code means that the URI of requested resource has been changed temporarily. Further changes in the URI might be made in the future. Therefore, this same URI should be used by the client in future requests.

## Client Error Responses
Code | Name | Description
---- | ---- | -----------
400 | Bad Request | The server could not understand the request due to invalid syntax.
401 | Unauthorized | Although the HTTP standard specifies "unauthorized", semantically this response means "unauthenticated". That is, the client must authenticate itself to get the requested response.
403 | Forbidden | The client does not have access rights to the content; that is, it is unauthorized, so the server is refusing to give the requested resource. Unlike 401, the client's identity is known to the server.
404 | Not Found | The server can not find the requested resource. This can also mean that the endpoint is valid but the resource itself does not exist. Servers may also send this response instead of 403 to hide the existence of a resource from an unauthorized client. This response code is probably the most famous one due to its frequent occurrence on the web.
405 | Method Not Allowed | The request method is known by the server but has been disabled and cannot be used.
406 | Not Acceptable | This response is sent when the web server, after performing server-driven content negotiation, doesn't find any content that conforms to the criteria given by the user agent.
409 | Conflict | This response is sent when a request conflicts with the current state of the server.
410 | Gone | This response is sent when the requested content has been permanently deleted from server, with no forwarding address.
413 | Payload Too Large | Request entity is larger than limits defined by our servers.
415 | Unsupported Media Type | The media format of the requested data is not supported by the server, so the server is rejecting the request.
423 | Locked  | The resource that is being accessed is locked.
429 | Too Many Requests | The user has sent too many requests in a given amount of time ("rate limiting").

## Server Error Responses
Code | Name | Description
---- | ---- | -----------
500 | Internal Server Error | The server has encountered a situation it doesn't know how to handle.
502 | Bad Gateway | This error response means that the server, while working as a gateway to get a response needed to handle the request, got an invalid response.
503 | Service Unavailable | The server is not ready to handle the request.

# Using Lists
There are many list endpoints available for Invenias core entities that can be leveraged to be used in Applications.

It's possible to POST a request body in a list endpoint request allowing you to define filters, sorting, grouping, column selection, pagination, and more.

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
Skip (optional) | 0 | Integer | Bypass a specified number of search results then return the remaining results.
Take (optional) | 0 | Integer | Specify the number of search results to return.
PageSize (optional) | 0 | Integer | Specify the number of search results to return.
PageIndex (optional) | 0 | Integer | Specify the PageIndex property to determine the index of the currently displayed page.
UsePaging (optional) | TRUE | Boolean | 
ReturnTotalCount (optional) | TRUE | Boolean | Displays the total number of items in the response.
ReturnTotalDatabaseItemCount (optional) | TRUE | Boolean | Displays the total number of items in the database.
ReturnUniqueValues (optional) | TRUE | Boolean | Returns an object containing an array of unique values queried from a given field (or values returned from an expression).
Select (optional) | [List] | Array | Specify an array of column names to be returned in the response.
Filter (optional) | [List] | Array | Filter items by column names and values.
CategoryFilter (optional) | [Record] | String | Filter items by relationally linked categories.
Sort (optional) | [List] | Array | Sort an array of items to sort by.
Group (optional) | [List] | Array | Group results together by a column in the array.
FormFactor (optional) | Any | String | Filter the items by the application used to create them 
DisplayViewId (optional) | 00000000-0000-0000-0000-000000000000 | String | Predefined arrays of columns based upon 'views' created in the Invenias Desktop application.
RequireGroupCount (optional) | TRUE | Boolean | 
IsFirstLoad (optional) | TRUE | Boolean | 
IncludeAdditionalValues (optional) | TRUE | Boolean | 
UseLookUpViewDefinition (optional) | TRUE | Boolean | 
IncludeDisplayViews (optional) | TRUE | Boolean | Displays the details of the predefined arrays of columns based upon 'views' created in the Invenias Desktop application for this entity.
IncludeAvailableColumns (optional) | TRUE | Boolean | Displays all of the column names available in the list for the entity.
IncludeCategories (optional) | TRUE | Boolean | Returns the category lists and categories enabled for the entity.


## Select
> Select Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/assignments/list' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
   "Select":[
      "AssignmentReferenceNumber",
      "CompanyId",
      "CompanyDisplayName",
      "FileAs"
   ]
}'

# Example Response:
{
    "Items": [
        {
            "AssignmentReferenceNumber": "A000002",
            "CompanyDisplayName": "Old Mutual",
            "CompanyId": {
                "Id": "ab4f3fce-3387-4e02-83d3-d99cab1df59e"
            },
            "FileAs": "Head of Sales & Marketing",
            "ItemType": "Assignments",
            "ItemId": "3f5007c4-3a76-42c0-af75-21e05a7fdeed",
            "OffLimitsStatus": "Off"
        }...
    ]
}
```
Select allows you to specify an array of column names to be returned in the response.

If you do not specify an array of column names in the request body the response will return the columns visible in the 'default' global display view for the entity you're requesting.

<aside class="notice">
Please note, for each item where there is 'null' data for a specified column, this column will not be returned in the response for those items.
</aside>

## Pagination
> Pagination Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/assignments/list' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
   "PageSize":1000,
   "PageIndex":0,
   "Select":[
      "AssignmentReferenceNumber",
      "CompanyId",
      "CompanyDisplayName",
      "FileAs"
   ]
   "Sort":[
      {
         "Selector":"AssignmentReferenceNumber",
         "Desc":false
      }
   ]
}'

# Example Response:
{
    "Items": [
        {
            "AssignmentReferenceNumber": "A000001",
            "CompanyDisplayName": "Old Mutual",
            "CompanyId": {
                "Id": "ab4f3fce-3387-4e02-83d3-d99cab1df59e"
            },
            "FileAs": "CFO",
            "ItemType": "Assignments",
            "ItemId": "ca866e18-6c8f-47b0-a76a-0dd29d498e6b",
            "OffLimitsStatus": "Off"
        }...
    ]
}
```

Pagination is a method of dividing web content into discrete pages, thus presenting content in a limited and digestible manner.

List endpoints have a maximum row limit of 1,000 rows per request. If you wanted to display the content in a paginated format or export all of the available results you will need to use these parameters.

`PageSize`, `PageIndex` and `UsePaging` are all used to paginate the dataset, as follows:

<ul>
	<li>PageSize - how many items you want in each page (max 1,000).</li>
	<li>PageIndex - the current page you want to retrieve.</li>
</ul>


<aside class="notice">
If you wish to get all of the available records in a list and wish to know how many requests you will need to make, simply make a request and use the ReturnTotalDatabaseItemCount parameter. Once the total number of items in the database has been determined you should know how many requests you need to make incrementing the page index each time to get all of the items in the list (e.g. TotalItemCount/PageSize = the number of requests required).
</aside>

## Group

> Group Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/assignments/list' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
    "Group": [
        {
            "IsExpanded": true,
            "Selector": "Status_lookup",
            "Desc": true
        }
    ],
    "RequireGroupCount": true
}'

# Example Response:
{
    "Groups": [
        {
            "Key": "Placement",
            "Count": 9
        },
        {
            "Key": "Active",
            "Count": 8
        }...
    ]
}
```
A group is a number of things that are located, gathered, or classed together.

Group can be leveraged to group results together by a column in the array.
## Count

> Count Example (cURL)


```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/assignments/list' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
   "ReturnTotalDatabaseItemCount": true,
   "Select":[
      "AssignmentReferenceNumber",
      "CompanyId",
      "CompanyDisplayName",
      "FileAs"
   ]
}'

# Example Response:
{
    "Items": [
        {
            "AssignmentReferenceNumber": "A000001",
            "CompanyDisplayName": "Old Mutual",
            "CompanyId": {
                "Id": "ab4f3fce-3387-4e02-83d3-d99cab1df59e"
            },
            "FileAs": "CFO",
            "ItemType": "Assignments",
            "ItemId": "ca866e18-6c8f-47b0-a76a-0dd29d498e6b",
            "OffLimitsStatus": "Off"
        }...
    ],
    "Paging": {
        "TotalItemCount": 1000,
        "TotalDatabaseItemCount": 5785
    }
}
```
 
ReturnTotalCount and ReturnTotalDatabaseItemCount are boolean parameters that allow you to get a count of records returned Vs. the total count of all items (of this entity) in your database.



## Comparison Operators
> Comparison Operator Example: 'in' (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/assignments/list' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
    "Select": [
        "AssignmentReferenceNumber",
        "CompanyId",
        "CompanyDisplayName",
        "FileAs"
    ],
    "Filter": [
        "Status_lookup",
        "in",
        [
            "Placement",
            "Active"
        ]
    ]
}'

# Example Response:
{
    "Items": [
        {
            "AssignmentReferenceNumber": "A000015",
            "CompanyDisplayName": "Ace Bakery",
            "CompanyId": {
                "Id": "26f18e44-7ce6-4109-9d3c-d2f2c4b20923"
            },
            "FileAs": "CT - Ace Bakery",
            "ItemType": "Assignments",
            "Status_lookup": "Active",
            "ItemId": "61e85df1-083d-43ae-841f-055303965039",
            "OffLimitsStatus": "Off"
        }...
    ]
}
```
> Comparison Operator Example: 'isnull' (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/assignments/list' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
    "Select": [
        "AssignmentReferenceNumber",
        "CompanyId",
        "CompanyDisplayName",
        "FileAs"
    ],
    "Filter": [
        "Status_lookup",
        "isnull"
    ]
}'

# Example Response:
{
    "Items": [
        {
            "AssignmentReferenceNumber": "A000039",
            "CompanyDisplayName": "Mitsubishi UFJ",
            "CompanyId": {
                "Id": "13e8432f-9a27-4f75-9dff-e6213419bcd8"
            },
            "FileAs": "Chief Operating Officer",
            "ItemType": "Assignments",
            "ItemId": "a0a9b874-7a63-44e3-b46e-1edebff970a5",
            "OffLimitsStatus": "Off"
        }...
    ]
}
```
> Comparison Operator Example: 'contains' (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/assignments/list' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
    "Select": [
        "RecordOwners"
    ],
    "Filter": [
        [
            "RecordOwners",
            "contains",
            "John"
        ]
    ],
    "Group": [
        {
            "IsExpanded": true,
            "Selector": "RecordOwners",
            "Desc": true
        }
    ],
    "RequireGroupCount": true
}'

# Example Response:
{
    "Groups": [
        {
            "Key": [
                {
                    "Id": "c76de083-f148-4ab4-8668-0ac80a444c26",
                    "ItemDisplayText": "John Doe"
                }
            ],
            "Count": 11
        },
        {
            "Key": [
                {
                    "Id": "e1a4174e-c78d-42e7-a9ea-b22381643681",
                    "ItemDisplayText": "Johnny Doe"
                }
            ],
            "Count": 8
        }
    ]
}
```

The list of available filter operators are as follows:

Operator | Description 
-------- | ----------- 
= | Valid for a column that contains text, numbers, or dates. Specify a single value. Results include only records where the data in the column matches the value in the filter.
> | Valid for a column that contains numbers or dates. Specify a single value. Results include only records where the data in the column is greater than the value in the filter.
>= | Valid for a column that contains numbers or dates. Specify a single value or multiple values. Results include only records where the data in the column is greater than or the same as the value in the filter.
< | Valid for a column that contains numbers or dates. Specify a single value. Results include only records where the data in the column is less than the value in the filter.
<= | Valid for a column that contains numbers or dates. Specify a single value or multiple values. Results include only records where the data in the column is less than or the same as the value in the filter.
<> | Valid for a column that contains text, numbers, or dates. Specify a single value. Results include only records where the data in the column does not match the value in the filter.
contains | Valid for a column that contains text, numbers, or dates. Specify a single value. Results include only records where the data in the column contains the value in the filter.
startswith | Valid for a column that contains text, numbers, or dates. Specify a single value. Results include only records where the data in the column begins with the value in the filter.
endswith | Valid for a column that contains text, numbers, or dates. Specify a single value. Results include only records where the data in the column ends with the value in the filter.
notcontains | Valid for a column that contains text, numbers, or dates. Specify a single value or multiple values. Results include only records where the data in the column does not contain the value in the filter.
isnull | Valid for a column that contains text, numbers, or dates. Do not specify a value. The operator tests only for the absence of data in the column. Results include only records where there is no data in the column.
isnullorempty | Valid for a column that contains text, numbers, or dates. Do not specify a value. The operator tests only for the absence of data or empty values in the column. Results include only records where there is no data in the column.
in | Valid for a column that contains text, numbers, or dates. Specify multiple values. Results include only records where the data in the column matches one of the values in the filter.


<aside class="notice">
All comparison operators above require 3 inputs as described above (property, operator, value), except, "isnull" and "isnullorempty". They are an exception to this rule as they do not require a value.
</aside>

## Logical Operators
> Logical Operator Example: 'and' (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/assignments/list' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
    "Select": [
        "AssignmentReferenceNumber",
        "CompanyId",
        "CompanyDisplayName",
        "FileAs",
        "Status_lookup",
        "EngagementType_lookup"
    ],
    "Filter": [
        [
            "Status_lookup",
            "=",
            "Completed"
        ],
        "and",
        [
            "EngagementType_lookup",
            "=",
            "Retained"
        ]
    ]
}'

# Example Response:
{
        "Items": [
        {
            "AssignmentReferenceNumber": "A000001",
            "CompanyDisplayName": "Old Mutual",
            "CompanyId": {
                "Id": "ab4f3fce-3387-4e02-83d3-d99cab1df59e"
            },
            "EngagementType_lookup": "Retained",
            "FileAs": "CFO",
            "ItemType": "Assignments",
            "Status_lookup": "Completed",
            "ItemId": "ca866e18-6c8f-47b0-a76a-0dd29d498e6b",
            "OffLimitsStatus": "Off"
        },
        {
            "AssignmentReferenceNumber": "A000014",
            "CompanyDisplayName": "Old Mutual",
            "CompanyId": {
                "Id": "ab4f3fce-3387-4e02-83d3-d99cab1df59e"
            },
            "EngagementType_lookup": "Retained",
            "FileAs": "COO",
            "ItemType": "Assignments",
            "Status_lookup": "Completed",
            "ItemId": "6a2fd500-073c-401e-aea5-574300d9b336",
            "OffLimitsStatus": "Off"
        }...
}
```

> Logical Operator Example: 'or' (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/assignments/list' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
    "Select": [
        "RecordOwners"
    ],
    "Filter": [
        [
            "RecordOwners",
            "contains",
            "John"
        ],
        "or",
        [
            "RecordOwners",
            "contains",
            "Jane"
        ]
    ],
    "Group": [
        {
            "IsExpanded": true,
            "Selector": "RecordOwners",
            "Desc": true
        }
    ],
    "RequireGroupCount": true
}'

# Example Response:
{
    "Groups": [
        {
            "Key": [
                {
                    "Id": "c76de083-f148-4ab4-8668-0ac80a444c26",
                    "ItemDisplayText": "Jane Doe"
                }
            ],
            "Count": 2
        },
        {
            "Key": [
                {
                    "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
                    "ItemDisplayText": "John Doe"
                }
            ],
            "Count": 11
        }
    ]
}
```
The `and` and `or` operators are used to filter records based on more than one condition:

<ul>
<li>The 'and' operator displays a record if all the conditions separated by 'and' are TRUE.</li>
<li>The 'or' operator displays a record if any of the conditions separated by 'or' is TRUE.</li>
</ul>

<aside class="notice">
Please note, the 'not' operator is not supported.
</aside>

## Sort
> Sort Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/assignments/list' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
   "Sort":[
      {
         "Selector":"AssignmentReferenceNumber",
         "Desc":false
      }
   ]
}'

# Example Response:
{
    "Items": [
        {
            "AssignmentReferenceNumber": "A000001",
            "CompanyDisplayName": "Old Mutual",
            "CompanyId": {
                "Id": "ab4f3fce-3387-4e02-83d3-d99cab1df59e"
            },
            "FileAs": "CFO",
            "ItemType": "Assignments",
            "ItemId": "ca866e18-6c8f-47b0-a76a-0dd29d498e6b",
            "OffLimitsStatus": "Off"
        }...
    ]
}
```
Sort An array of objects to sort by, using the following format {"Selector":"ColumnName","Desc": true}.

# Data Management
Data management policies are used to ensure that an organization's data and information assets are managed consistently and used properly. Invenias provides functionality allowing our customers to apply policies to any numbers of fields (within a predefined list) to core entities allowing them to either make filling them in a `preferred` or `compulsory` task. 

If one or more fields within an entity are marked as `compulsory` you will not be able to update the entity (or create a new one) unless you populate the compulsory field(s) via the most applicable method(s).

<aside class="warning">
Failure to respect compulsory data management policies when crating or updating records will result in a failed request with a 500 HTTP code. The response may include a message like "Invenias.Model.Services.Services.Services.DataManagementService.ValidatePositionCompanyPolicy".
</aside>

<aside class="notice">
Please note, data management policies are not available for the Programme entity.
</aside>

## GET /api/v1/datamanagement/people/rules
> Sort Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/datamanagement/people/rules' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer {token}'

# Example Response:
{
    "Fields": [
        {
            "FieldName": "DateOfBirth",
            "FieldPolicy": "Normal",
            "Label": "Date Of Birth"
        },
        {
            "FieldName": "Company",
            "FieldPolicy": "Compulsory",
            "Label": "Company"
        },
        {
            "FieldName": "Email1Address",
            "FieldPolicy": "Normal",
            "Label": "Email"
        },
        {
            "FieldName": "JobTitle",
            "FieldPolicy": "Normal",
            "Label": "Job Title"
        }...
    ]
}
```

This endpoint will display any data management polices applied to this entity type.
### HTTP Request
`https://{subdomain}.invenias.com/api/v1/datamanagement/people/rules`

## GET /api/v1/datamanagement/companies/rules
> Sort Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/datamanagement/companies/rules' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer {token}'

# Example Response:
{
    "Fields": [
        {
            "FieldName": "Email2Address",
            "FieldPolicy": "Normal",
            "Label": "Email 2"
        },
        {
            "FieldName": "BusinessPhone",
            "FieldPolicy": "Compulsory",
            "Label": "Business Phone"
        },
        {
            "FieldName": "Email1Address",
            "FieldPolicy": "Preferred",
            "Label": "Email"
        },
        {
            "FieldName": "Email3Address",
            "FieldPolicy": "Normal",
            "Label": "Email 3"
        }...
    ]
}
```
This endpoint will display any data management polices applied to this entity type.
### HTTP Request
`https://{subdomain}.invenias.com/api/v1/datamanagement/companies/rules`

## GET /api/v1/datamanagement/assignment/rules
> Sort Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/datamanagement/assignment/rules' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer {token}'

# Example Response:
{
    "Fields": [
        {
            "FieldName": "EngagementType",
            "FieldPolicy": "Compulsory",
            "Label": "Engagement Type"
        },
        {
            "FieldName": "InternalRef",
            "FieldPolicy": "Normal",
            "Label": "Internal Ref"
        },
        {
            "FieldName": "Company",
            "FieldPolicy": "Compulsory",
            "Label": "Company Name"
        },
        {
            "FieldName": "Location",
            "FieldPolicy": "Normal",
            "Label": "Location"
        },
        {
            "FieldName": "ExternalRef",
            "FieldPolicy": "Normal",
            "Label": "External Ref"
        },
        {
            "FieldName": "AssignmentStatus",
            "FieldPolicy": "Compulsory",
            "Label": "Assignment Status"
        }...
    ]
}
```
This endpoint will display any data management polices applied to this entity type.
### HTTP Request
`https://{subdomain}.invenias.com/api/v1/datamanagement/assignment/rules`

# Settings
## GET /api/v1/settings/{key}
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/settings/AssignmentCandidateStatus' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer {token}'

# Example Response:
{
    "SettingName": "AssignmentCandidateStatus",
    "Settings": {
        "ItemReferences": [
            {
                "Id": "9af12b92-baae-4a5c-89a3-c9d9ab0a4db9",
                "DisplayTitle": "Target Candidate",
                "ItemDisplayText": "Target Candidate",
                "ItemType": "LookupListEntries",
                "StatusGroupName": "InitialValue"
            },
            {
                "Id": "f54fa1c4-3964-4815-96cb-689cd34cf6d6",
                "DisplayTitle": "Left Message",
                "ItemDisplayText": "Left Message",
                "ItemType": "LookupListEntries",
                "StatusGroupName": "ContactOutstanding"
            },
            {
                "Id": "86d21315-1268-4162-b68f-175f6b739ca3",
                "DisplayTitle": "Sent Email",
                "ItemDisplayText": "Sent Email",
                "ItemType": "LookupListEntries",
                "StatusGroupName": "ContactOutstanding"
            },
            {
                "Id": "1eb6d90b-1738-4b67-9e90-3091135b7357",
                "DisplayTitle": "In Discussion",
                "ItemDisplayText": "In Discussion",
                "ItemType": "LookupListEntries",
                "StatusGroupName": "Contact"
            },
            {
                "Id": "c08d00a0-76d1-4678-a49c-d543dd19f46d",
                "DisplayTitle": "Consultant Interview",
                "ItemDisplayText": "Consultant Interview",
                "ItemType": "LookupListEntries",
                "StatusGroupName": "ConsultantInterview"
            },
            {
                "Id": "dfd64726-7e88-45a7-b3f8-49c00929a985",
                "DisplayTitle": "Rejected by Consultant",
                "ItemDisplayText": "Rejected by Consultant",
                "ItemType": "LookupListEntries",
                "StatusGroupName": "Rejected"
            },
            {
                "Id": "26586f57-9232-40c4-ad68-23b72a9f741d",
                "DisplayTitle": "Rejected by Client",
                "ItemDisplayText": "Rejected by Client",
                "ItemType": "LookupListEntries",
                "StatusGroupName": "Rejected"
            }...
        ]
    }
}
```
> Returns a list of the items available used to track where a candidate is in the search process.

The Invenias system has many default and customizable enumerations. When designing an integration it may be necessary to know the values of the items in a collection for any given enumeration. The GET /api/v1/settings/{key} endpoint will return the items for any enumeration name defined in the `{key}`.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/settings/{key}`

Parameter | Default | Description
--------- | ------- | -----------
key | [required] | The display name of the enumeration.

### Enumerations
<ul>
<li>AllCompaniesProgressStatuses</li>
<li>AllPeopleProgressStatuses</li>
<li>AppointmentActionTypes</li>
<li>AssignmentAdStatus</li>
<li>AssignmentAdvertEmploymentType</li>
<li>AssignmentCandidateGroups</li>
<li>AssignmentCandidateStatus</li>
<li>AssignmentClientRole</li>
<li>AssignmentCompanyStatus</li>
<li>AssignmentEmploymentType</li>
<li>AssignmentFeeType</li>
<li>AssignmentLostReasons</li>
<li>AssignmentReferralStatus</li>
<li>AssignmentResearchAssignmentStatus</li>
<li>AssignmentResearchProgrammeStatus</li>
<li>AssignmentStatus</li>
<li>AssignmentWeighting</li>
<li>CandidateContractTypes</li>
<li>ClientContractTypes</li>
<li>ClientSource</li>
<li>CommissionType</li>
<li>CompanyClientStatus</li>
<li>CompanyRelationship</li>
<li>CompanySize</li>
<li>CompanySource</li>
<li>CompanyTeamRole</li>
<li>CompanyType</li>
<li>ConsentEventTypes</li>
<li>Currencies</li>
<li>DataManagementPriorities</li>
<li>DefaultCandidateProgressStatus</li>
<li>DocumentType</li>
<li>DurationPeriod</li>
<li>EducationQualifications</li>
<li>EmailActionTypes</li>
<li>EngagementType</li>
<li>FeedbackActionTypes</li>
<li>GoogleAnalyticsKey</li>
<li>InterimRateSettings</li>
<li>InterviewActionTypes</li>
<li>MaritalStatus</li>
<li>Nationality</li>
<li>NonExecPackageSettings</li>
<li>NoteActionTypes</li>
<li>OfferRejectedReasons</li>
<li>PackageRatePeriod</li>
<li>PaymentFrequencies</li>
<li>PaymentType</li>
<li>PermanentPackageSettings</li>
<li>PersonAvailability</li>
<li>PersonCandidateStatus</li>
<li>PersonClientStatus</li>
<li>PersonCustom1</li>
<li>PersonCustom2</li>
<li>PersonCustom3</li>
<li>PersonOfferStatus</li>
<li>PersonPlacementStatus</li>
<li>PersonPlacementType</li>
<li>PersonPositionStatus</li>
<li>PersonPositionType</li>
<li>PlacementCancellationReasons</li>
<li>PlacementOrigin</li>
<li>PositionRole</li>
<li>PriorityTypes</li>
<li>ProgrammeTeamRole</li>
<li>ProgressTrackingActionTypes</li>
<li>ReferenceFormat</li>
<li>ReferenceProgressStatus</li>
<li>ReferenceType</li>
<li>SentCurriculumVitaeActionTypes</li>
<li>SentDocumentActionTypes</li>
<li>Sex</li>
<li>TaskActionTypes</li>
<li>TeamRole</li>
<li>TelephoneActionTypes</li>
</ul>

## GET/api/v1/countries
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/countries' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer {token}'

# Example Response:
{
    "Items": [
        {
            "CountryCode": "AF",
            "CountryName": "AFGHANISTAN"
        },
        {
            "CountryCode": "AX",
            "CountryName": "ALAND ISLANDS"
        },
        {
            "CountryCode": "AL",
            "CountryName": "ALBANIA"
        },
        {
            "CountryCode": "DZ",
            "CountryName": "ALGERIA"
        },
        {
            "CountryCode": "AS",
            "CountryName": "AMERICAN SAMOA"
        }...
    ]
}
```
The GET/api/v1/countries endpoint will get a list of all of the country codes and names supported by the Invenias systems.
# Duplicate Records
The Invenias REST API has two endpoints available to help flag 'People' and 'Company'  type entities that <i>may</i> already exist within the database. By utilizing these endpoints it will help maintain the integrity of the data in the database, allowing the developer to either resolve updates to an existing entity or create a new one entirely.

## GET /api/v1/duplicates/people
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/duplicates/people?request.personName=Glen%20Chamberlain&request.emailAddress=someemailaddress@gmail.com&request.pageIndex=0&request.pageSize=10' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer {token}'

# Example Response:
[
    {
        "PersonName": "Glen Chamberlain",
        "CompanyName": "Internal Dev Database",
        "JobTitle": "Technical Operations Engineer",
        "EmailAddress1": "someemailaddress@gmail.com",
        "EmailAddress2": "email2@aaa.com",
        "IsExactEmailMatch": false,
        "MatchingFields": [
            {
                "FieldName": "PersonName",
                "Value": "Glen Chamberlain"
            }
        ],
        "ItemId": "7d68cd5d-cb47-4710-8641-2bf3a57a2e80",
        "DisplaySummary": "Glen Chamberlain (Chamberlain), Internal Dev Database, Technical Operations Engineer",
        "DisplayName": "Glen Chamberlain (Chamberlain)",
        "ItemType": "People",
    },
    {
        "PersonName": "Glen Chamberlain",
        "CompanyName": "Internal Dev Database",
        "JobTitle": "Partner",
        "EmailAddress1": "someemailaddress@gmail.com",
        "EmailAddress2": "email2@aaa.com",
        "EmailAddress3": "email3@aaa.com",
        "IsExactEmailMatch": false,
        "MatchingFields": [
            {
                "FieldName": "PersonName",
                "Value": "Glen Chamberlain"
            }
        ],
        "ItemId": "9edf2ce8-39ad-4b3c-a36f-3872ff43e5f8",
        "DisplaySummary": "Glen Chamberlain, Internal Dev Database, Partner",
        "DisplayName": "Glen Chamberlain",
        "ItemType": "People",
    }...
]
```
The GET /api/v1/duplicates/people endpoint is designed to flag potentially existing 'People' types entities based upon the search terms passed via the `request.personName` and `request.emailAddress` parameters.

For performance, it's <b>strongly</b> advised to pass the simplest search term possible via the 'request.personName'. The reason for this is that the search term will be split using the spaces as delimiters and parameters will be created in every conceivable permutation for comparison. The more parameters that are created and comparatively references in the server-side query the longer it will take to return a response.

### Name Components
This endpoint does not comparatively reference all of the naming component fields for 'People' type entities. Please do not include salutations, suffixes, and prefixes in the 'request.personName' parameter. For a full list of the naming components leveraged by this endpoint please see below:
<ul>
    <li>PersonFirstName</li>
    <li>PersonMiddleName</li>
    <li>PersonFamilyName</li>
    <li>PersonNickName</li>
    <li>PersonMaidenName</li>
</ul>

### Email Address
This endpoint does not comparatively reference all of the email fields for 'People' type entities. It does not reference the custom email address fields. For a full list of the email address fields leveraged by this endpoint please see below:
<ul>
    <li>PersonEmailAddress1</li>
    <li>PersonEmailAddress2</li>
    <li>PersonEmailAddress3</li>
</ul>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/duplicates/people?request.personName=Glen%20Chamberlain&request.emailAddress=inveniasreporting%2Bdemo%40gmail.com&request.pageIndex=0&request.pageSize=10`

Parameter | Default | Description
--------- | ------- | -----------
request.personName (optional) | | Specify the desired search term for name components to the query to be executed on the server.
request.emailAddress (optional) | | Specify desired search term for email address fields to the query to be executed on the server.
request.pageIndex (optional) | | Specify the PageIndex property to determine the index of the currently displayed page.
request.pageSize (optional) | | Specify the number of search results to return.

<aside class="notice">
Please note, it may be optional to pass a value in both the 'request.personName' and 'request.emailAddress' parameters individually, you must input a value into at least one of them to make a successful request.
</aside>

## GET /api/v1/duplicates/companies

# FAQ
## Frequently asked questions
This section provides answers to common questions from both developers and customers:

### What is an API?
An application programming interface (API) is a set of routines, protocols, and tools for building software applications.

### What is the rate limit of the API?
We use a fixed-window rate-limiting strategy, up to `3000` API calls can be made at any interval within a 5-minute window. For more information on rate limits please see [here](https://bullhorn.github.io/invenias-api-docs/#rate-limiting).

### Will, you make changes to your API that will leave my integration unusable?
Whenever we make a change to the API we try to do so in an additive way that won't break existing integrations. However, occasionally things can change in a way that isn't backward compatible. In this event, communication will be provided.

### Can you help us with the coding?
REST is an industry standard that is not dependant on any programming language or technology apart from HTTP. Our support team may not be able to help you with your coding problems as they are not aware of all the possible ways to implement a web service client and have limited knowledge about your platform architecture or technology stack. Therefore, Invenias may not be able to guarantee and support your code.

### How do I filter lists?
Yes, it's possible to leverage both comparison and logical operators to filter lists. You can find more information [here](https://bullhorn.github.io/invenias-api-docs/#using-lists).

### Can I get a sandbox environment to develop in?
Yes, it's possible to arrange for a clone of a tenant to be created to be used as a sandbox; however, this comes at a cost. Please speak with your Invenias account manager for more information.

# Troubleshooting
This section has troubleshooting procedures for the following problems that people may encounter with the Invenias API.

## Bad Request (400) Response Code
A `400` Bad Request error indicates that the data provided in an API call is unrecognizable. The culprit is often broken or invalid JSON, perhaps a parse error. For the most part, 400 errors have already passed authentication, so the error response message will provide more specific information about the error.

A `400` status code means that the server could not process an API request due to invalid syntax. A few possibilities why this might happen are:
<ul>
    <li>A typo or mistake while building out the request manually, such as mistyping the API endpoint, a header name or value, or a query parameter. This can also be caused if the request is missing a required query parameter, or the query parameter value is invalid or missing.</li>
    <li>An invalid JSON body, for example, missing a set of double-quotes, or a comma. Please validate your code using a linter to avoid issues.</li>
    <li>The request is missing authentication information, or the Authorization header provided could not be validated.</li>
</ul>

Please review every single piece of text in the request, ensuring that there are no typos in the endpoint, headers (name and values), and body. If you copied and pasted any part of your API request, pay extra attention that they don't include any mistakes or random characters that could be causing an issue.

<aside class="warning">
If this response is accompanied by the message "invalid_client" the 'Content-Type' header is incorrect or is missing from the request. Please ensure the 'Content-Type' header is declared in your request and the value is set to 'application/x-www-form-urlencoded'.
</aside>

## Unauthorized (401) Response Code
The 401 Unauthorized status code is returned when the API request is missing authentication credentials or the credentials provided are invalid. This may happen for any of the following reasons:
<ul>
    <li>The username has been changed.</li>
    <li>The user account password has been reset.</li>
    <li>The user account has been disabled.</li>
    <li>The token has expired.</li>
    <li>The license has been removed from the user account.</li>
    <li>The user accounts permission group has been changed and it's no longer in the 'System Administrator' group.</li>
    <li>The user account has been disabled by our BTO team due to exceeding 9000 429 responses within a 5 minute period. Please check your logs for 429 responses. Please note, in this event, the primary contact of the Invenias customer will be notified of this action.</li>
</ul>

## Forbidden (403) Response Code
The `403` error status code indicates the server understood the request but refuses to authorize it.

If authentication credentials were provided in the request, the server considers them insufficient to grant access. For example, the user might be trying to access account-level information that's only viewable by the account administrator, and the credentials being passed on the API request are for a regular user account.

Another reason this status code might be returned is in case the user did not request an API access token with the correct permissions.

To fix the API call for those two situations, make sure that the credentials you are using have the access level required by the endpoint, or that the access token has the correct permissions.


Please note, if you are receiving `404` or `403` response codes from our servers there's a chance the Application has expired. In this event please see [here](https://bullhorn.github.io/invenias-api-docs/#renewing-an-application) for more information on what to do. If you're unsure if your Application has expired or not please email our support team using support@invenias.com and they will be able to check for you.

## Not Found (404) Response Code
The `404` error status code indicates that the REST API can't map the client's URI to a resource but may be available in the future. ... This status code is used when our server does not wish to reveal exactly why the request has been refused, or when no other response is applicable.

Please note, if you are receiving `404` or `403` response codes from our servers there's a chance the Application has expired. In this event please see [here](https://bullhorn.github.io/invenias-api-docs/#renewing-an-application) for more information on what to do. If you're unsure if your Application has expired or not please email our support team using support@invenias.com and they will be able to check for you.

## Internal Server Error (500) Response Code
The `500` Internal Server Error server error response code indicates that the server encountered an unexpected condition that prevented it from fulfilling the request.

Please note, the database may have data management policies enabled on the entity types you're trying to work with. Please see [here](https://bullhorn.github.io/invenias-api-docs/#data-management) for more information on data management policies.



