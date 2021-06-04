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


> Example Response (JSON)


```shell
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

Before you can use the `POST /api/v1/thirdpartyapplications` endpoint, you need to enter an `api_key` in the field at the top right-hand corner of the Swagger page. To do this simply double click into the `api_key` field in the header to automatically generate one. This will prompt you to log in if you're not already logged in. 

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


> Example Response (JSON)


```shell
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
```


> Example Response (JSON)


```shell
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

You can find out how many requests you have remaining by checking the value in the `X-Request-Quota-Remaining` response header.

<img src="images\quotaremaining.png" alt="X-Request-Quota-Remaining" class="inline"/>
<i>Figure 1. API Call Response Headers - X-Request-Quota-Remaining</i>

Once the `X-Request-Quota-Remaining reaches` 0, all succeeding requests made will return `429` errors, until the current rate limit window resets. At this time your Application should wait for the limit to become available again.

<aside class="warning">
Failure to add adequate mechanisms to your Application instructing it to wait after receiving a 429 error may result in the user account used to authenticate the requests being disabled by our BTO team. This would happen should it generate 9000 (or more) 429 errors within any given 5-minute period. The account will remain disabled until their satisfied that steps have been taken to prevent the problem from occurring again.
</aside>

# HTTP Response Status Codes
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

<aside class="notice">
Please note, there is a row limit of <b>1000</b> rows per call when using a List endpoint.
</aside>

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
Skip (optional) | 0 | Integer | Bypass a specified number of search results then return the remaining results.
Take (optional) | 0 | Integer | Specify the number of search results to return.
PageSize (optional) | 0 | Integer | Specify the number of search results to return.
PageIndex (optional) | 0 | Integer | Specify the PageIndex property to determine the index of the currently displayed page.
UsePaging (optional) | true | Boolean | 
ReturnTotalCount (optional) | true | Boolean | Displays the total number of items in the response.
ReturnTotalDatabaseItemCount (optional) | true | Boolean | Displays the total number of items in the database.
ReturnUniqueValues (optional) | true | Boolean | Returns an object containing an array of unique values queried from a given field (or values returned from an expression).
Select (optional) | [List] | Array | Specify an array of column names to be returned in the response.
Filter (optional) | [List] | Array | Filter items by column names and values.
CategoryFilter (optional) | [Record] | String | Filter items by relationally linked categories.
Sort (optional) | [List] | Array | Sort an array of items to sort by.
Group (optional) | [List] | Array | Group results together by a column in the array.
FormFactor (optional) | Any | String | Filter the items by the application used to create them 
DisplayViewId (optional) | 00000000-0000-0000-0000-000000000000 | String | Predefined arrays of columns based upon 'views' created in the Invenias Desktop application.
RequireGroupCount (optional) | true | Boolean | 
IsFirstLoad (optional) | true | Boolean | 
IncludeAdditionalValues (optional) | true | Boolean | <u>Some</u> list endpoints contain a nested array of columns named `AdditionalValues`, this parameter can be used to specify if they should be visible in the response body
UseLookUpViewDefinition (optional) | true | Boolean | 
IncludeDisplayViews (optional) | true | Boolean | Displays the details of the predefined arrays of columns based upon 'views' created in the Invenias Desktop application for this entity.
IncludeAvailableColumns (optional) | true | Boolean | Displays all of the column names available in the list for the entity.
IncludeCategories (optional) | true | Boolean | Returns the category lists and categories enabled for the entity.


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
```

> Example Response (JSON)

```shell
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
```

> Example Response (JSON)

```shell
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
```

> Example Response (JSON)

```shell
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
```

> Example Response (JSON)

```shell
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
```

> Example Response (JSON)

```shell
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
```

> Example Response (JSON)

```shell
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
```

> Example Response (JSON)

```shell
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
```

> Example Response (JSON)

```shell
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
```

> Example Response (JSON)

```shell
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
<li>The 'and' operator displays a record if all the conditions separated by 'and' are true.</li>
<li>The 'or' operator displays a record if any of the conditions separated by 'or' is true.</li>
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
```

> Example Response (JSON)

```shell
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
```

> Example Response (JSON)

```shell
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
```

> Example Response (JSON)

```shell
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
```

> Example Response (JSON)

```shell
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
```

> Example Response (JSON)

```shell
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
```

> Example Response (JSON)

```shell
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

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/countries`

<aside class="warning">
The Invenias systems use the USPS standard for country names. When adding entries to Address fields the value must exist within the GET/api/v1/countries list and it must be entered entirely in uppercase (e.g. UNITED KINGDOM, FRANCE, INDIA, etc...). Failure to follow these guidelines will result in a null value in this field when creating or updating entities.
</aside>

# Lookup Lists
A Lookup List is a list of values which have either been used or which are suggested for use in a field. In other words, a Lookup List can be built from values keyed by users into a field and / or it can be pre-loaded with values.


## POST /api/v1/lookuplists/list
> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/lookuplists/list' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{}'
```

> Example Response (JSON)

```shell
{
    "Items": [
        {
            "ItemType": "LookupLists",
            "Name": "DataManagementPriorities",
            "ItemId": "b06f6dea-e72f-44f0-b311-f0697b1e17aa",
            "OffLimitsStatus": "Off"
        },
        {
            "ItemType": "LookupLists",
            "Name": "TeamRole",
            "ItemId": "b906ef4d-3ee2-4a31-8ff4-f32d7b75ffaa",
            "OffLimitsStatus": "Off"
        },
        {
            "ItemType": "LookupLists",
            "Name": "PriorityTypes",
            "ItemId": "5a294ba8-427c-4ca6-bd23-fb47356e176b",
            "OffLimitsStatus": "Off"
        },
        {
            "ItemType": "LookupLists",
            "Name": "AssignmentClientRole",
            "ItemId": "793b73af-7ad8-4d1c-b9ba-fdc6a5a168b7",
            "OffLimitsStatus": "Off"
        },
        {
            "ItemType": "LookupLists",
            "Name": "NoteActionTypes",
            "ItemId": "e103fc3c-e797-408f-bcec-ff4dc4f965aa",
            "OffLimitsStatus": "Off"
        }...
    ]...
}
```

This endpoint can be used to get the names of all of the available Lookup Lists in the database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/lookuplists/list`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
Skip (optional) | 0 | Integer | Bypass a specified number of search results then return the remaining results.
Take (optional) | 0 | Integer | Specify the number of search results to return.
PageSize (optional) | 0 | Integer | Specify the number of search results to return.
PageIndex (optional) | 0 | Integer | Specify the PageIndex property to determine the index of the currently displayed page.
UsePaging (optional) | true | Boolean | 
ReturnTotalCount (optional) | true | Boolean | Displays the total number of items in the response.
ReturnTotalDatabaseItemCount (optional) | true | Boolean | Displays the total number of items in the database.
ReturnUniqueValues (optional) | true | Boolean | Returns an object containing an array of unique values queried from a given field (or values returned from an expression).
Select (optional) | [List] | Array | Specify an array of column names to be returned in the response.
Filter (optional) | [List] | Array | Filter items by column names and values.
CategoryFilter (optional) | [Record] | String | Filter items by relationally linked categories.
Sort (optional) | [List] | Array | Sort an array of items to sort by.
Group (optional) | [List] | Array | Group results together by a column in the array.
FormFactor (optional) | Any | String | Filter the items by the application used to create them 
DisplayViewId (optional) | 00000000-0000-0000-0000-000000000000 | String | Predefined arrays of columns based upon 'views' created in the Invenias Desktop application.
RequireGroupCount (optional) | true | Boolean | 
IsFirstLoad (optional) | true | Boolean | 
IncludeAdditionalValues (optional) | true | Boolean | <u>Some</u> list endpoints contain a nested array of columns named `AdditionalValues`, this parameter can be used to specify if they should be visible in the response body
UseLookUpViewDefinition (optional) | true | Boolean | 
IncludeDisplayViews (optional) | true | Boolean | Displays the details of the predefined arrays of columns based upon 'views' created in the Invenias Desktop application for this entity.
IncludeAvailableColumns (optional) | true | Boolean | Displays all of the column names available in the list for the entity.
IncludeCategories (optional) | true | Boolean | Returns the category lists and categories enabled for the entity.

## POST /api/v1/lookuplists/{id}/entries/list
> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/lookuplists/{id}/entries/list' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{}'
```

> Example Response (JSON)

```shell
{
    "Items": [
        {
            "IsActive": true,
            "ItemType": "LookupListEntries",
            "LookupListId": {
                "Id": "dc10b565-1f05-4561-a394-8019a7f760aa"
            },
            "Name": "Placement",
            "OrderIndex": 0,
            "ItemId": "3775f6d6-af16-45c3-bfa5-3e28d6fde30d",
            "OffLimitsStatus": "Off"
        },
        {
            "IsActive": true,
            "ItemType": "LookupListEntries",
            "LookupListId": {
                "Id": "dc10b565-1f05-4561-a394-8019a7f760aa"
            },
            "Name": "Active",
            "OrderIndex": 1,
            "ItemId": "12a6b6f2-f121-47d4-870c-2c93fee84a71",
            "OffLimitsStatus": "Off"
        },
        {
            "IsActive": true,
            "ItemType": "LookupListEntries",
            "LookupListId": {
                "Id": "dc10b565-1f05-4561-a394-8019a7f760aa"
            },
            "Name": "Cancelled",
            "OrderIndex": 4,
            "ItemId": "71b671b0-83b9-4411-8bde-1f8453635ef0",
            "OffLimitsStatus": "Off"
        },
        {
            "IsActive": true,
            "ItemType": "LookupListEntries",
            "LookupListId": {
                "Id": "dc10b565-1f05-4561-a394-8019a7f760aa"
            },
            "Name": "Completed",
            "OrderIndex": 5,
            "ItemId": "34e89aee-df20-4699-bb9a-e436c3dfc44f",
            "OffLimitsStatus": "Off"
        }...
    ]...
}
```
This endpoint can be used to get the values withing a specific Lookup List.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/lookuplists/{id}/entries/list`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for a Lookup List.
Skip (optional) | 0 | Integer | Bypass a specified number of search results then return the remaining results.
Take (optional) | 0 | Integer | Specify the number of search results to return.
PageSize (optional) | 0 | Integer | Specify the number of search results to return.
PageIndex (optional) | 0 | Integer | Specify the PageIndex property to determine the index of the currently displayed page.
UsePaging (optional) | true | Boolean | 
ReturnTotalCount (optional) | true | Boolean | Displays the total number of items in the response.
ReturnTotalDatabaseItemCount (optional) | true | Boolean | Displays the total number of items in the database.
ReturnUniqueValues (optional) | true | Boolean | Returns an object containing an array of unique values queried from a given field (or values returned from an expression).
Select (optional) | [List] | Array | Specify an array of column names to be returned in the response.
Filter (optional) | [List] | Array | Filter items by column names and values.
CategoryFilter (optional) | [Record] | String | Filter items by relationally linked categories.
Sort (optional) | [List] | Array | Sort an array of items to sort by.
Group (optional) | [List] | Array | Group results together by a column in the array.
FormFactor (optional) | Any | String | Filter the items by the application used to create them 
DisplayViewId (optional) | 00000000-0000-0000-0000-000000000000 | String | Predefined arrays of columns based upon 'views' created in the Invenias Desktop application.
RequireGroupCount (optional) | true | Boolean | 
IsFirstLoad (optional) | true | Boolean | 
IncludeAdditionalValues (optional) | true | Boolean | <u>Some</u> list endpoints contain a nested array of columns named `AdditionalValues`, this parameter can be used to specify if they should be visible in the response body
UseLookUpViewDefinition (optional) | true | Boolean | 
IncludeDisplayViews (optional) | true | Boolean | Displays the details of the predefined arrays of columns based upon 'views' created in the Invenias Desktop application for this entity.
IncludeAvailableColumns (optional) | true | Boolean | Displays all of the column names available in the list for the entity.
IncludeCategories (optional) | true | Boolean | Returns the category lists and categories enabled for the entity.

## POST /api/v1/lookuplists/{id}/entries
> Request Model Example (JSON)

```shell
{
  "Name": "Lost"
}
```

> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/lookuplists/{id}/entries' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
  "Name": "Lost"
}'
```

> Example Response (JSON)

```shell
{
    "Items": [
        {
            "IsActive": true,
            "ItemType": "LookupListEntries",
            "LookupListId": {
                "Id": "dc10b565-1f05-4561-a394-8019a7f760aa"
            },
            "Name": "Lost",
            "OrderIndex": 10,
            "ItemId": "2927c7dd-ab34-4686-8504-03e31d36e898",
            "OffLimitsStatus": "Off"
        }
    ]
}
```
This endpoint can be used to create new entries in a specific Lookup List.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/lookuplists/{id}/entries/list`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for the Lookup List.
request | [required] | String | The request model used to create a new value in the Lookup List.

## PUT /api/v1/lookuplists/{id}/entries/{itemId}

# Duplicates
The Invenias REST API has two endpoints available to help flag 'People' and 'Company'  type entities that <i>may</i> already exist within the database. By utilizing these endpoints it will help maintain the integrity of the data in the database, allowing the developer to either resolve updates to an existing entity or create a new one entirely.

## GET /api/v1/duplicates/people
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/duplicates/people?request.personName=John%20Doe&request.emailAddress=someemailaddress@gmail.com&request.pageIndex=0&request.pageSize=10' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
[
    {
        "PersonName": "Johnny Doe",
        "CompanyName": "Microsoft",
        "JobTitle": "Technical Operations Engineer",
        "EmailAddress1": "someemailaddress@gmail.com",
        "EmailAddress2": "email2@aaa.com",
        "IsExactEmailMatch": true,
        "MatchingFields": [
            {
                "FieldName": "PersonName",
                "Value": "John Doe"
            }
        ],
        "ItemId": "7d68cd5d-cb47-4710-8641-2bf3a57a2e80",
        "DisplaySummary": "John Doe (Doe), Microsoft, Technical Operations Engineer",
        "DisplayName": "John Doe (Doe)",
        "ItemType": "People",
    },
    {
        "PersonName": "John Doe",
        "CompanyName": "Apple Inc.",
        "JobTitle": "Partner",
        "EmailAddress1": "email2@aaa.com",
        "EmailAddress2": "email3@aaa.com",
        "IsExactEmailMatch": false,
        "MatchingFields": [
            {
                "FieldName": "PersonName",
                "Value": "John Doe"
            }
        ],
        "ItemId": "9edf2ce8-39ad-4b3c-a36f-3872ff43e5f8",
        "DisplaySummary": "John Doe, Apple Inc., Partner",
        "DisplayName": "John Doe",
        "ItemType": "People",
    }...
]
```
The GET /api/v1/duplicates/people endpoint is designed to flag potentially existing 'People' type entities based upon the search terms passed via the `request.personName` and `request.emailAddress` parameters.

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
`https://{subdomain}.invenias.com/api/v1/duplicates/people?request.personName=John%20Doe&request.emailAddress=someemailaddress%2Bdemo%40gmail.com&request.pageIndex=0&request.pageSize=10`

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
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/duplicates/companies?request.companyName=Invenias&request.pageIndex=0&request.pageSize=10' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
[
    {
        "IsPlaceOfStudy": false,
        "Location": "Default",
        "IsExactMatch": false,
        "MatchingFields": [
            {
                "FieldName": "CompanyName",
                "Value": "Invenias by Bullhorn"
            }
        ],
        "ItemId": "7fbd6130-a867-4901-9284-06ac9c6dfdd1",
        "DisplaySummary": "Invenias by Bullhorn, Default",
        "DisplayName": "Invenias by Bullhorn",
        "ItemType": "Companies"
    },
    {
        "IsPlaceOfStudy": false,
        "Location": "Default",
        "IsExactMatch": false,
        "MatchingFields": [
            {
                "FieldName": "CompanyName",
                "Value": "Invenias Limited"
            }
        ],
        "ItemId": "0e5c71d2-bdd4-43ab-90ff-69c9824eca39",
        "DisplaySummary": "Invenias Limited, Default",
        "DisplayName": "Invenias Limited",
        "ItemType": "Companies"
    }
]
```
The GET /api/v1/duplicates/companies endpoint is designed to flag potentially existing 'Company' type entities based upon the search terms passed via the `request.companyName` parameter.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/duplicates/companies?request.companyName=Invenias&request.pageIndex=0&request.pageSize=10`

Parameter | Default | Description
--------- | ------- | -----------
request.companyName | [required] | Specify the desired search term for the query to be executed on the server.
request.pageIndex (optional) | | Specify the PageIndex property to determine the index of the currently displayed page.
request.pageSize (optional) | | Specify the number of search results to return.

# Quick Search
The quick search endpoints allow you to pass a search term to get a list of entities that match <b>OR</b> partially match the term for many entity types.

<aside class="notice">
The key distinction between the 'Quick Search' and 'Search' endpoints is that the 'Search' endpoints allow you to select columns, filter, and group the results. The 'Quick Search' endpoints do not.
</aside>

## GET /api/v2/quicksearch/people
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v2/quicksearch/people?request.searchTerm=John&request.pageIndex=0&request.pageSize=10' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
[
    {
        "PersonName": "John Doe",
        "CompanyName": "Microsoft",
        "JobTitle": "Technical Operations Engineer",
        "ItemId": "a9504f72-4f20-4d4a-a8d1-1a18ed571c9d",
        "DisplaySummary": "John Doe, Microsoft, Technical Operations Engineer",
        "DisplayName": "John Doe",
        "ItemType": "People"
    },
    {
        "PersonName": "Johnny Doe",
        "CompanyName": "",
        "JobTitle": "Senior Data Engineer",
        "ItemId": "90b38639-096c-4745-9750-a379379c6d15",
        "DisplaySummary": "John Doe, Senior Data Engineer",
        "DisplayName": "John Doe",
        "ItemType": "People"
    },
    {
        "PersonName": "Jane Doe",
        "CompanyName": "John Corp.",
        "JobTitle": "Partner",
        "ItemId": "6e5a99e7-0389-4a16-8ca6-459ca4197690",
        "DisplaySummary": "John Doe, Janen Corp.",
        "DisplayName": "John Doe",
        "ItemType": "People"
    }...
]...

```
This endpoint allows you to pass a search term to get a list of entities where the `PersonName` string matches <b>OR</b> partially matches the search term.

### HTTP Request
`https://{subdomain}.invenias.com/api/v2/quicksearch/people?request.searchTerm=John&request.pageIndex=0&request.pageSize=10`

Parameter | Default | Description
--------- | ------- | -----------
extendedSearch (optional) | true | Boolean | Specify if you wish to extend the search to the Person entities default Position entities `Job Title`.
request.searchTerm | [required] | Specify the desired search term for the query to be executed on the server.
request.pageIndex (optional) | | Specify the PageIndex property to determine the index of the currently displayed page.
request.pageSize (optional) | | Specify the number of search results to return.

## GET /api/v1/quicksearch/companies
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/quicksearch/companies?request.searchTerm=Inv&request.pageIndex=0&request.pageSize=10' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
[
    {
        "LocationName": "Default",
        "ItemId": "d4fcae19-8c29-49cd-be11-be82b9b2a366",
        "DisplaySummary": "Invenias by Bullhorn, Default",
        "DisplayName": "Invenias by Bullhorn",
        "ItemType": "Companies"
    },
    {
        "LocationName": "Default",
        "ItemId": "0e5c71d2-bdd4-43ab-90ff-69c9824eca39",
        "DisplaySummary": "Invenias Limited, Default",
        "DisplayName": "Invenias Limited",
        "ItemType": "Companies"
    },
    {
        "LocationName": "",
        "ItemId": "956e89e3-5e89-4704-b6cc-1ff27ec24e4e",
        "DisplaySummary": "Investec",
        "DisplayName": "Investec",
        "ItemType": "Companies"
    }...
]...

```
This endpoint allows you to pass a search term to get a list of entities where the `DisplayName` string matches <b>OR</b> partially matches the search term.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/quicksearch/companies?request.searchTerm=Inv&request.pageIndex=0&request.pageSize=10`

Parameter | Default | Description
--------- | ------- | -----------
request.searchTerm | [required] | Specify the desired search term for the query to be executed on the server.
request.pageIndex (optional) | | Specify the PageIndex property to determine the index of the currently displayed page.
request.pageSize (optional) | | Specify the number of search results to return.

## GET /api/v1/quicksearch/educationalorganisations

> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/quicksearch/educationalorganisations?request.searchTerm=Aca&request.pageIndex=0&request.pageSize=10' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
[
    {
        "LocationName": "Default",
        "ItemId": "11db8c22-81de-4a82-9b47-dc4b54ad415a",
        "DisplaySummary": "Academy of Contemporary Music, Default",
        "DisplayName": "Academy of Contemporary Music",
        "ItemType": "Companies"
    },
    {
        "LocationName": "Guildford",
        "ItemId": "7bc5b01f-c36a-4828-bea5-e3af7f5882e1",
        "DisplaySummary": "Academy of Contemporary Music",
        "DisplayName": "Academy of Contemporary Music",
        "ItemType": "Companies"
    }...
]...

```
This endpoint allows you to pass a search term to get a list of entities where the `DisplayName` string matches <b>OR</b> partially matches the search term.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/quicksearch/educationalorganisations?request.searchTerm=Aca&request.pageIndex=0&request.pageSize=10`

Parameter | Default | Description
--------- | ------- | -----------
request.searchTerm | [required] | Specify the desired search term for the query to be executed on the server.
request.pageIndex (optional) | | Specify the PageIndex property to determine the index of the currently displayed page.
request.pageSize (optional) | | Specify the number of search results to return.

## GET /api/v1/quicksearch/assignments

> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/quicksearch/assignments?request.searchTerm=Vice&request.pageIndex=0&request.pageSize=10' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
[
    {
        "CompanyName": "Old Mutual",
        "ItemId": "bbd2c96c-00bc-4ef9-aae7-9ec88b9d80cd",
        "DisplaySummary": "Vice President, Securities, Old Mutual",
        "DisplayName": "Vice President, Securities",
        "ItemType": "Assignments"
    },
    {
        "CompanyName": "JP Morgan",
        "ItemId": "f6284f92-1cb0-4d67-8eed-d8b021155752",
        "DisplaySummary": "Executive Vice President, JP Morgan",
        "DisplayName": "Executive Vice President",
        "ItemType": "Assignments"
    }...
]...

```
This endpoint allows you to pass a search term to get a list of entities where the `CompanyName` string matches <b>OR</b> partially matches the search term.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/quicksearch/assignments?request.searchTerm=Vice&request.pageIndex=0&request.pageSize=10`

Parameter | Default | Description
--------- | ------- | -----------
request.searchTerm | [required] | Specify the desired search term for the query to be executed on the server.
request.pageIndex (optional) | | Specify the PageIndex property to determine the index of the currently displayed page.
request.pageSize (optional) | | Specify the number of search results to return.

## GET /api/v1/quicksearch/programmes

> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/quicksearch/programmes?request.searchTerm=Net&request.pageIndex=0&request.pageSize=10' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
[
    {
        "ProgrammeTypeId": "886f1f8f-e651-4a80-9a86-63ef3600337d",
        "ProgrammeType": "Business Development",
        "ItemId": "8f537861-7709-4ef2-bbec-68db11b19ade",
        "DisplaySummary": "Network Groups",
        "DisplayName": "Network Groups",
        "ItemType": "Programmes"
    },
    {
        "ProgrammeTypeId": "886f1f8f-e651-4a80-9a86-63ef3600337d",
        "ProgrammeType": "Business Development",
        "ItemId": "a2adc7f7-973a-4751-99bd-c9ce7ed3c504",
        "DisplaySummary": "Networking Breakfast Event 2017 (Business Development)",
        "DisplayName": "Networking Breakfast Event 2017",
        "ItemType": "Programmes"
    },
    {
        "ProgrammeTypeId": "e1c7f9b0-03b2-4aca-a103-ae75480d1315",
        "ProgrammeType": "Marketing Event",
        "ItemId": "470efcfd-f4ff-4155-b098-e5f5ef80aee6",
        "DisplaySummary": "Networking Lunch Event 2017 (Marketing Event)",
        "DisplayName": "Networking Lunch Event 2017",
        "ItemType": "Programmes"
    }
]...

```
This endpoint allows you to pass a search term to get a list of entities where the `DisplayName` string matches <b>OR</b> partially matches the search term.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/quicksearch/programmes?request.searchTerm=Net&request.pageIndex=0&request.pageSize=10`

Parameter | Default | Description
--------- | ------- | -----------
request.searchTerm | [required] | Specify the desired search term for the query to be executed on the server.
request.pageIndex (optional) | | Specify the PageIndex property to determine the index of the currently displayed page.
request.pageSize (optional) | | Specify the number of search results to return.


## GET /api/v1/quicksearch/professionalusers

> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/quicksearch/professionalusers?request.searchTerm=Jane&request.pageIndex=0&request.pageSize=10' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
[
  {
    "PersonName": "Jane Doe",
    "CompanyName": "Internal Dev Database",
    "JobTitle": "Partner",
    "ItemId": "a9504f72-4f20-4d4a-a8d1-1a18ed571c9d",
    "DisplaySummary": "Jane Doe, Internal Dev Database, Partner",
    "DisplayName": "Jane Doe",
    "ItemType": "People",
  },
  {
    "PersonName": "Jane Doe",
    "CompanyName": "Internal Dev Database",
    "JobTitle": "Technical Operations Engineer",
    "ItemId": "7d68cd5d-cb47-4710-8641-2bf3a57a2e80",
    "DisplaySummary": "Jane Doe (Doe), Internal Dev Database, Technical Operations Engineer",
    "DisplayName": "Jane Doe (Doe)",
    "ItemType": "People",
  },
  {
    "PersonName": "Acacia Richards",
    "CompanyName": "Internal Dev Database",
    "JobTitle": "Jane",
    "ItemId": "40c52385-c8f7-4092-83b3-46a84209ed45",
    "DisplaySummary": "Acacia Richards, Internal Dev Database, Jane",
    "DisplayName": "Acacia Richards",
    "ItemType": "People",
  }...
]...

```
This endpoint allows you to pass a search term to get a list of entities where the `PersonName` string matches <b>OR</b> partially matches the search term.

<aside class="notice">
Please note, each Professional User will have both a Person and User type entity. This endpoint specifically returns information related to their Person entity.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/quicksearch/quicksearch/professionalusers?request.searchTerm=Jane&request.pageIndex=0&request.pageSize=10`

Parameter | Default | Description
--------- | ------- | -----------
request.searchTerm | [required] | Specify the desired search term for the query to be executed on the server.
request.pageIndex (optional) | | Specify the PageIndex property to determine the index of the currently displayed page.
request.pageSize (optional) | | Specify the number of search results to return.

## GET /api/v1/quicksearch/clientusers

> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/quicksearch/clientusers?request.searchTerm=Jane&request.pageIndex=0&request.pageSize=10' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
[
  {
    "PersonName": "Jane Doe",
    "CompanyName": "Lloyds Banking Group",
    "JobTitle": "Chief Executive Officer",
    "ItemId": "e3617a59-2cd1-4eea-974d-aaa9ad4742ab",
    "DisplaySummary": "Jane Doe, Lloyds Banking Group, Chief Executive Officer",
    "DisplayName": "Jane Doe",
    "ItemType": "People"
  }...
]...

```
This endpoint allows you to pass a search term to get a list of entities where the `PersonName` string matches <b>OR</b> partially matches the search term.

<aside class="notice">
Please note, the definition of a Client User is a Person type entity who has been added to the 'Client' team in one or more Assignment Type entities, the Invenias Client feature is enabled on the tenant and at least one Assignment has been shared with them.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/quicksearch/clientusers?request.searchTerm=Jane&request.pageIndex=0&request.pageSize=10`

Parameter | Default | Description
--------- | ------- | -----------
request.searchTerm | [required] | Specify the desired search term for the query to be executed on the server.
request.pageIndex (optional) | | Specify the PageIndex property to determine the index of the currently displayed page.
request.pageSize (optional) | | Specify the number of search results to return.

# Search
The Invenias REST API has endpoints available for most entity types allowing you to find entities that correspond to keywords or characters specified in the search term.

<aside class="warning">
Please note, the minimum charater length for the search term is 3 characters.
</aside>

<aside class="notice">
The key distinction between the 'Quick Search' and 'Search' endpoints is that the 'Search' endpoints allow you to select columns, filter, and group the results. The 'Quick Search' endpoints do not.
</aside>

## POST /api/v1/search/users

> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/search/users?request.searchTerm=Jane' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
    "Select": [
        "Disabled",
        "IsLicenseAssigned",
        "DisplayFileAs",
        "UserId"
    ],
    "Filter": [
        "IsLicenseAssigned",
        "=",
        "true"
    ]
}'
```

> Example Response (JSON)

```shell
{
    "Items": [
        {
            "Disabled": false,
            "DisplayFileAs": "Jane Doe",
            "FileAs": "Jane Doe",
            "IsLicenseAssigned": true,
            "ItemType": "People",
            "UserId": {
                "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5"
            },
            "ItemId": "a9504f72-4f20-4d4a-a8d1-1a18ed571c9d",
            "OffLimitsStatus": "Off"
        },
        {
            "Disabled": false,
            "DisplayFileAs": "Jane Doe",
            "FileAs": "Ms Jane Doe B.A. (Hons) (Doe) (J)",
            "IsLicenseAssigned": true,
            "ItemType": "People",
            "UserId": {
                "Id": "c49c61bb-d321-452c-b77d-62ef6661e1f4"
            },
            "ItemId": "7d68cd5d-cb47-4710-8641-2bf3a57a2e80",
            "OffLimitsStatus": "Off"
        }
    ]
}
```
This endpoint allows you to pass a search term and select, group, and filter Professional User entities to get a list of entities where the `DisplayFileAs` string matches <b>OR</b> partially matches the search term.

<aside class="notice">
Please note, each Professional User will have both a Person and User type entity. This endpoint will return information related to both the Person and User entities.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/search/users?request.searchTerm=Jane`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
request.searchTerm | [required] | String | Specify the desired search term for the query to be executed on the server.
Skip (optional) | 0 | Integer | Bypass a specified number of search results then return the remaining results.
Take (optional) | 0 | Integer | Specify the number of search results to return.
PageSize (optional) | 0 | Integer | Specify the number of search results to return.
PageIndex (optional) | 0 | Integer | Specify the PageIndex property to determine the index of the currently displayed page.
UsePaging (optional) | true | Boolean | 
ReturnTotalCount (optional) | true | Boolean | Displays the total number of items in the response.
ReturnTotalDatabaseItemCount (optional) | true | Boolean | Displays the total number of items in the database.
ReturnUniqueValues (optional) | true | Boolean | Returns an object containing an array of unique values queried from a given field (or values returned from an expression).
Select (optional) | [List] | Array | Specify an array of column names to be returned in the response.
Filter (optional) | [List] | Array | Filter items by column names and values.
CategoryFilter (optional) | [Record] | String | Filter items by relationally linked categories.
Sort (optional) | [List] | Array | Sort an array of items to sort by.
Group (optional) | [List] | Array | Group results together by a column in the array.
FormFactor (optional) | Any | String | Filter the items by the application used to create them 
DisplayViewId (optional) | 00000000-0000-0000-0000-000000000000 | String | Predefined arrays of columns based upon 'views' created in the Invenias Desktop application.
RequireGroupCount (optional) | true | Boolean | 
IsFirstLoad (optional) | true | Boolean | 
IncludeAdditionalValues (optional) | true | Boolean | <u>Some</u> list endpoints contain a nested array of columns named `AdditionalValues`, this parameter can be used to specify if they should be visible in the response body
UseLookUpViewDefinition (optional) | true | Boolean | 
IncludeDisplayViews (optional) | true | Boolean | Displays the details of the predefined arrays of columns based upon 'views' created in the Invenias Desktop application for this entity.
IncludeAvailableColumns (optional) | true | Boolean | Displays all of the column names available in the list for the entity.
IncludeCategories (optional) | true | Boolean | Returns the category lists and categories enabled for the entity.

## POST /api/v1/search/people

> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/search/people?request.searchTerm=Jane' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
    "Select": [
        "DisplayFileAs",
        "PositionJobTitle",
        "CompanyDisplayName",
        "TotalCandidateAssignments"
    ],
    "Filter": [
        [
            "CompanyDisplayName",
            "contains",
            "Int"
        ],
        "and",
        [
            "TotalCandidateAssignments",
            ">",
            0
        ]
    ],
       "Sort":[
      {
         "Selector":"TotalCandidateAssignments",
         "Desc":true
      }
   ]
}'
```

> Example Response (JSON)

```shell
{
    "Items": [
        {
            "CompanyDisplayName": "Invenias by Bullhorn",
            "DisplayFileAs": "Jane Doe",
            "ItemType": "People",
            "PositionJobTitle": "Technical Operations Engineer",
            "TotalCandidateAssignments": 3,
            "ItemId": "7d68cd5d-cb47-4710-8641-2bf3a57a2e80",
            "OffLimitsStatus": "Off"
        },
        {
            "CompanyDisplayName": "Bullhorn",
            "DisplayFileAs": "Jane Doe",
            "ItemType": "People",
            "PositionJobTitle": "Technical Operations Engineer",
            "TotalCandidateAssignments": 1,
            "ItemId": "72a0fabd-7c04-4436-878f-b3ff68461c51",
            "OffLimitsStatus": "Off"
        }...
    ]...
}
```
This endpoint allows you to pass a search term and select, group, and filter People type entities to get a list of entities where the `DisplayFileAs` string matches <b>OR</b> partially matches the search term.


### HTTP Request
`https://{subdomain}.invenias.com/api/v1/search/people?request.searchTerm=John`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
request.searchTerm | [required] | String | Specify the desired search term for the query to be executed on the server.
extendedSearch (optional) | true | Boolean | Specify if you wish to extend the search to the Person entities default Position entities `Job Title`.
Skip (optional) | 0 | Integer | Bypass a specified number of search results then return the remaining results.
Take (optional) | 0 | Integer | Specify the number of search results to return.
PageSize (optional) | 0 | Integer | Specify the number of search results to return.
PageIndex (optional) | 0 | Integer | Specify the PageIndex property to determine the index of the currently displayed page.
UsePaging (optional) | true | Boolean | 
ReturnTotalCount (optional) | true | Boolean | Displays the total number of items in the response.
ReturnTotalDatabaseItemCount (optional) | true | Boolean | Displays the total number of items in the database.
ReturnUniqueValues (optional) | true | Boolean | Returns an object containing an array of unique values queried from a given field (or values returned from an expression).
Select (optional) | [List] | Array | Specify an array of column names to be returned in the response.
Filter (optional) | [List] | Array | Filter items by column names and values.
CategoryFilter (optional) | [Record] | String | Filter items by relationally linked categories.
Sort (optional) | [List] | Array | Sort an array of items to sort by.
Group (optional) | [List] | Array | Group results together by a column in the array.
FormFactor (optional) | Any | String | Filter the items by the application used to create them 
DisplayViewId (optional) | 00000000-0000-0000-0000-000000000000 | String | Predefined arrays of columns based upon 'views' created in the Invenias Desktop application.
RequireGroupCount (optional) | true | Boolean | 
IsFirstLoad (optional) | true | Boolean | 
IncludeAdditionalValues (optional) | true | Boolean | <u>Some</u> list endpoints contain a nested array of columns named `AdditionalValues`, this parameter can be used to specify if they should be visible in the response body
UseLookUpViewDefinition (optional) | true | Boolean | 
IncludeDisplayViews (optional) | true | Boolean | Displays the details of the predefined arrays of columns based upon 'views' created in the Invenias Desktop application for this entity.
IncludeAvailableColumns (optional) | true | Boolean | Displays all of the column names available in the list for the entity.
IncludeCategories (optional) | true | Boolean | Returns the category lists and categories enabled for the entity.

## POST /api/v1/search/companies

> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/search/companies?request.searchTerm=Old' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
    "Select": [
        "FileAs",
        "ClientStatus_lookup",
        "CompanyDisplayName",
        "TotalNumberOfAssignments"
    ],
       "Sort":[
      {
         "Selector":"TotalNumberOfAssignments",
         "Desc":true
      }
   ]
}'
```

> Example Response (JSON)

```shell
{
    "Items": [
        {
            "FileAs": "Old Mutual",
            "ItemType": "Companies",
            "TotalNumberOfAssignments": 6,
            "ItemId": "ab4f3fce-3387-4e02-83d3-d99cab1df59e",
            "OffLimitsStatus": "Off"
        },
        {
            "FileAs": "Old Mutual Plc.",
            "ItemType": "Companies",
            "TotalNumberOfAssignments": 0,
            "ItemId": "676339da-8e18-497f-9f27-52740dfc4fe0",
            "OffLimitsStatus": "Off"
        }
    ]...
}...
```
This endpoint allows you to pass a search term and select, group, and filter Company type entities to get a list of entities where the `FileAs` string matches <b>OR</b> partially matches the search term.


### HTTP Request
`https://{subdomain}.invenias.com/api/v1/search/companies?request.searchTerm=Old`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
request.searchTerm | [required] | String | Specify the desired search term for the query to be executed on the server.
Skip (optional) | 0 | Integer | Bypass a specified number of search results then return the remaining results.
Take (optional) | 0 | Integer | Specify the number of search results to return.
PageSize (optional) | 0 | Integer | Specify the number of search results to return.
PageIndex (optional) | 0 | Integer | Specify the PageIndex property to determine the index of the currently displayed page.
UsePaging (optional) | true | Boolean | 
ReturnTotalCount (optional) | true | Boolean | Displays the total number of items in the response.
ReturnTotalDatabaseItemCount (optional) | true | Boolean | Displays the total number of items in the database.
ReturnUniqueValues (optional) | true | Boolean | Returns an object containing an array of unique values queried from a given field (or values returned from an expression).
Select (optional) | [List] | Array | Specify an array of column names to be returned in the response.
Filter (optional) | [List] | Array | Filter items by column names and values.
CategoryFilter (optional) | [Record] | String | Filter items by relationally linked categories.
Sort (optional) | [List] | Array | Sort an array of items to sort by.
Group (optional) | [List] | Array | Group results together by a column in the array.
FormFactor (optional) | Any | String | Filter the items by the application used to create them 
DisplayViewId (optional) | 00000000-0000-0000-0000-000000000000 | String | Predefined arrays of columns based upon 'views' created in the Invenias Desktop application.
RequireGroupCount (optional) | true | Boolean | 
IsFirstLoad (optional) | true | Boolean | 
IncludeAdditionalValues (optional) | true | Boolean | <u>Some</u> list endpoints contain a nested array of columns named `AdditionalValues`, this parameter can be used to specify if they should be visible in the response body
UseLookUpViewDefinition (optional) | true | Boolean | 
IncludeDisplayViews (optional) | true | Boolean | Displays the details of the predefined arrays of columns based upon 'views' created in the Invenias Desktop application for this entity.
IncludeAvailableColumns (optional) | true | Boolean | Displays all of the column names available in the list for the entity.
IncludeCategories (optional) | true | Boolean | Returns the category lists and categories enabled for the entity.


## POST /api/v1/search/educationalorganisations
> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/search/educationalorganisations?request.searchTerm=Aca' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{}'
```

> Example Response (JSON)

```shell
{
    "Items": [
        {
            "FileAs": "Academy of Contemporary Music",
            "ItemType": "Companies",
            "RecordOwners": [
                {
                    "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
                    "ItemDisplayText": "Glen Chamberlain"
                }
            ],
            "ItemId": "11db8c22-81de-4a82-9b47-dc4b54ad415a",
            "OffLimitsStatus": "Off"
        },
        {
            "FileAs": "Academy of Contemporary Music",
            "ItemType": "Companies",
            "RecordOwners": [
                {
                    "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
                    "ItemDisplayText": "Glen Chamberlain"
                }
            ],
            "ItemId": "7bc5b01f-c36a-4828-bea5-e3af7f5882e1",
            "OffLimitsStatus": "Off"
        }...
    ]...
}
```
This endpoint allows you to pass a search term and select, group, and filter Company type entities to get a list of entities flagged as a 'Place of Study' where the `FileAs` string matches <b>OR</b> partially matches the search term.


### HTTP Request
`https://{subdomain}.invenias.com/api/v1/search/educationalorganisations?request.searchTerm=Aca`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
request.searchTerm | [required] | String | Specify the desired search term for the query to be executed on the server.
Skip (optional) | 0 | Integer | Bypass a specified number of search results then return the remaining results.
Take (optional) | 0 | Integer | Specify the number of search results to return.
PageSize (optional) | 0 | Integer | Specify the number of search results to return.
PageIndex (optional) | 0 | Integer | Specify the PageIndex property to determine the index of the currently displayed page.
UsePaging (optional) | true | Boolean | 
ReturnTotalCount (optional) | true | Boolean | Displays the total number of items in the response.
ReturnTotalDatabaseItemCount (optional) | true | Boolean | Displays the total number of items in the database.
ReturnUniqueValues (optional) | true | Boolean | Returns an object containing an array of unique values queried from a given field (or values returned from an expression).
Select (optional) | [List] | Array | Specify an array of column names to be returned in the response.
Filter (optional) | [List] | Array | Filter items by column names and values.
CategoryFilter (optional) | [Record] | String | Filter items by relationally linked categories.
Sort (optional) | [List] | Array | Sort an array of items to sort by.
Group (optional) | [List] | Array | Group results together by a column in the array.
FormFactor (optional) | Any | String | Filter the items by the application used to create them 
DisplayViewId (optional) | 00000000-0000-0000-0000-000000000000 | String | Predefined arrays of columns based upon 'views' created in the Invenias Desktop application.
RequireGroupCount (optional) | true | Boolean | 
IsFirstLoad (optional) | true | Boolean | 
IncludeAdditionalValues (optional) | true | Boolean | <u>Some</u> list endpoints contain a nested array of columns named `AdditionalValues`, this parameter can be used to specify if they should be visible in the response body
UseLookUpViewDefinition (optional) | true | Boolean | 
IncludeDisplayViews (optional) | true | Boolean | Displays the details of the predefined arrays of columns based upon 'views' created in the Invenias Desktop application for this entity.
IncludeAvailableColumns (optional) | true | Boolean | Displays all of the column names available in the list for the entity.
IncludeCategories (optional) | true | Boolean | Returns the category lists and categories enabled for the entity.

## POST /api/v1/search/assignments
> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/search/assignments?request.searchTerm=Head' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Select": [
        "AssignmentReferenceNumber",
        "CompanyId",
        "CompanyDisplayName",
        "FileAs",
        "Status_lookup",
        "EngagementType_lookup"
    ]
}'
```

> Example Response (JSON)

```shell
{
    "Items": [
        {
            "AssignmentReferenceNumber": "A000002",
            "CompanyDisplayName": "Old Mutual",
            "CompanyId": {
                "Id": "ab4f3fce-3387-4e02-83d3-d99cab1df59e"
            },
            "EngagementType_lookup": "Retained",
            "FileAs": "Head of Sales & Marketing",
            "ItemType": "Assignments",
            "Status_lookup": "On Hold",
            "ItemId": "3f5007c4-3a76-42c0-af75-21e05a7fdeed",
            "OffLimitsStatus": "Off"
        },
        {
            "AssignmentReferenceNumber": "A000004",
            "CompanyDisplayName": "Deutsche Bank",
            "CompanyId": {
                "Id": "5d2c28b5-032b-4e17-ac9c-a8bfa2d94dff"
            },
            "EngagementType_lookup": "Retained",
            "FileAs": "Head of Marketing, EMEA",
            "ItemType": "Assignments",
            "Status_lookup": "Placement",
            "ItemId": "ed25ed9b-062b-4e10-a055-6fb024fe53b2",
            "OffLimitsStatus": "Off"
        }...
    ]
}
```
This endpoint allows you to pass a search term and select, group, and filter Assignment type entities to get a list of entities where the `FileAs` string matches <b>OR</b> partially matches the search term.


### HTTP Request
`https://{subdomain}.invenias.com/api/v1/search/assignments?request.searchTerm=Head`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
request.searchTerm | [required] | String | Specify the desired search term for the query to be executed on the server.
Skip (optional) | 0 | Integer | Bypass a specified number of search results then return the remaining results.
Take (optional) | 0 | Integer | Specify the number of search results to return.
PageSize (optional) | 0 | Integer | Specify the number of search results to return.
PageIndex (optional) | 0 | Integer | Specify the PageIndex property to determine the index of the currently displayed page.
UsePaging (optional) | true | Boolean | 
ReturnTotalCount (optional) | true | Boolean | Displays the total number of items in the response.
ReturnTotalDatabaseItemCount (optional) | true | Boolean | Displays the total number of items in the database.
ReturnUniqueValues (optional) | true | Boolean | Returns an object containing an array of unique values queried from a given field (or values returned from an expression).
Select (optional) | [List] | Array | Specify an array of column names to be returned in the response.
Filter (optional) | [List] | Array | Filter items by column names and values.
CategoryFilter (optional) | [Record] | String | Filter items by relationally linked categories.
Sort (optional) | [List] | Array | Sort an array of items to sort by.
Group (optional) | [List] | Array | Group results together by a column in the array.
FormFactor (optional) | Any | String | Filter the items by the application used to create them 
DisplayViewId (optional) | 00000000-0000-0000-0000-000000000000 | String | Predefined arrays of columns based upon 'views' created in the Invenias Desktop application.
RequireGroupCount (optional) | true | Boolean | 
IsFirstLoad (optional) | true | Boolean | 
IncludeAdditionalValues (optional) | true | Boolean | <u>Some</u> list endpoints contain a nested array of columns named `AdditionalValues`, this parameter can be used to specify if they should be visible in the response body
UseLookUpViewDefinition (optional) | true | Boolean | 
IncludeDisplayViews (optional) | true | Boolean | Displays the details of the predefined arrays of columns based upon 'views' created in the Invenias Desktop application for this entity.
IncludeAvailableColumns (optional) | true | Boolean | Displays all of the column names available in the list for the entity.
IncludeCategories (optional) | true | Boolean | Returns the category lists and categories enabled for the entity.

## POST /api/v1/search/programmes

> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/search/programmes?request.searchTerm=Network' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Select": [
        "ProgrammeReferenceNumber",
        "Name",
        "ProgrammeType",
        "ProgrammeStatusDisplay",
        "RecordOwners"
    ]
}'
```

> Example Response (JSON)

```shell
{
    "Items": [
        {
            "ItemType": "Programmes",
            "Name": "Networking Lunch 2020",
            "ProgrammeReferenceNumber": "CA000020",
            "ProgrammeType": "Business Development",
            "ItemId": "f252a77b-713f-4214-ae4f-ac644d3b6057",
            "OffLimitsStatus": "Off"
        },
        {
            "ItemType": "Programmes",
            "Name": "Networking Breakfast Event 2017",
            "ProgrammeReferenceNumber": "CA000006",
            "ProgrammeStatusDisplay": "Active",
            "ProgrammeType": "Business Development",
            "RecordOwners": [
                {
                    "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
                    "ItemDisplayText": "Glen Chamberlain"
                }
            ],
            "ItemId": "a2adc7f7-973a-4751-99bd-c9ce7ed3c504",
            "OffLimitsStatus": "Off"
        },
        {
            "ItemType": "Programmes",
            "Name": "Networking Lunch Event 2017",
            "ProgrammeReferenceNumber": "CA000007",
            "ProgrammeType": "Marketing Event",
            "RecordOwners": [
                {
                    "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
                    "ItemDisplayText": "Glen Chamberlain"
                }
            ],
            "ItemId": "470efcfd-f4ff-4155-b098-e5f5ef80aee6",
            "OffLimitsStatus": "Off"
        }
    ]
}
```
This endpoint allows you to pass a search term and select, group, and filter Programme type entities to get a list of entities where the `Name` string matches <b>OR</b> partially matches the search term.


### HTTP Request
`https://{subdomain}.invenias.com/api/v1/search/programmes?request.searchTerm=Network`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
request.searchTerm | [required] | String | Specify the desired search term for the query to be executed on the server.
Skip (optional) | 0 | Integer | Bypass a specified number of search results then return the remaining results.
Take (optional) | 0 | Integer | Specify the number of search results to return.
PageSize (optional) | 0 | Integer | Specify the number of search results to return.
PageIndex (optional) | 0 | Integer | Specify the PageIndex property to determine the index of the currently displayed page.
UsePaging (optional) | true | Boolean | 
ReturnTotalCount (optional) | true | Boolean | Displays the total number of items in the response.
ReturnTotalDatabaseItemCount (optional) | true | Boolean | Displays the total number of items in the database.
ReturnUniqueValues (optional) | true | Boolean | Returns an object containing an array of unique values queried from a given field (or values returned from an expression).
Select (optional) | [List] | Array | Specify an array of column names to be returned in the response.
Filter (optional) | [List] | Array | Filter items by column names and values.
CategoryFilter (optional) | [Record] | String | Filter items by relationally linked categories.
Sort (optional) | [List] | Array | Sort an array of items to sort by.
Group (optional) | [List] | Array | Group results together by a column in the array.
FormFactor (optional) | Any | String | Filter the items by the application used to create them 
DisplayViewId (optional) | 00000000-0000-0000-0000-000000000000 | String | Predefined arrays of columns based upon 'views' created in the Invenias Desktop application.
RequireGroupCount (optional) | true | Boolean | 
IsFirstLoad (optional) | true | Boolean | 
IncludeAdditionalValues (optional) | true | Boolean | <u>Some</u> list endpoints contain a nested array of columns named `AdditionalValues`, this parameter can be used to specify if they should be visible in the response body
UseLookUpViewDefinition (optional) | true | Boolean | 
IncludeDisplayViews (optional) | true | Boolean | Displays the details of the predefined arrays of columns based upon 'views' created in the Invenias Desktop application for this entity.
IncludeAvailableColumns (optional) | true | Boolean | Displays all of the column names available in the list for the entity.
IncludeCategories (optional) | true | Boolean | Returns the category lists and categories enabled for the entity.

## POST /api/v1/search/documents

> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/search/documents?request.searchTerm=Jane' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "PageSize": 50,
    "PageIndex": 0,
    "Select": [
        "AttachmentName",
        "DocumentExtension",
        "IsDefaultCv"
    ],
    "Sort": [
        {
            "Selector": "IsDefaultCv",
            "Desc": true
        }
    ]
}'
```

> Example Response (JSON)

```shell
{
    "Items": [
        {
            "AttachmentName": "Jane_Doe_CV.pdf",
            "DocumentExtension": "pdf",
            "ItemId": "0247367e-3e54-4edd-ba80-c9b16a2b6f02",
            "ItemType": "Documents",
            "OffLimitsStatus": "Off"
        },
        {
            "AttachmentName": "Jane_D_CV.docx",
            "DocumentExtension": "docx",
            "ItemId": "5679cee4-e14a-4fc8-8018-d1c12511bd51",
            "ItemType": "Documents",
            "OffLimitsStatus": "Off"
        }
    ]
}
```
This endpoint allows you to pass a search term and select, group, and filter Document type entities to get a list of entities where the `AttachmentName` or `Creator` string matches <b>OR</b> partially matches the search term.


### HTTP Request
`https://{subdomain}.invenias.com/api/v1/search/documents?request.searchTerm=Jane`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
request.searchTerm | [required] | String | Specify the desired search term for the query to be executed on the server.
Skip (optional) | 0 | Integer | Bypass a specified number of search results then return the remaining results.
Take (optional) | 0 | Integer | Specify the number of search results to return.
PageSize (optional) | 0 | Integer | Specify the number of search results to return.
PageIndex (optional) | 0 | Integer | Specify the PageIndex property to determine the index of the currently displayed page.
UsePaging (optional) | true | Boolean | 
ReturnTotalCount (optional) | true | Boolean | Displays the total number of items in the response.
ReturnTotalDatabaseItemCount (optional) | true | Boolean | Displays the total number of items in the database.
ReturnUniqueValues (optional) | true | Boolean | Returns an object containing an array of unique values queried from a given field (or values returned from an expression).
Select (optional) | [List] | Array | Specify an array of column names to be returned in the response.
Filter (optional) | [List] | Array | Filter items by column names and values.
CategoryFilter (optional) | [Record] | String | Filter items by relationally linked categories.
Sort (optional) | [List] | Array | Sort an array of items to sort by.
Group (optional) | [List] | Array | Group results together by a column in the array.
FormFactor (optional) | Any | String | Filter the items by the application used to create them 
DisplayViewId (optional) | 00000000-0000-0000-0000-000000000000 | String | Predefined arrays of columns based upon 'views' created in the Invenias Desktop application.
RequireGroupCount (optional) | true | Boolean | 
IsFirstLoad (optional) | true | Boolean | 
IncludeAdditionalValues (optional) | true | Boolean | <u>Some</u> list endpoints contain a nested array of columns named `AdditionalValues`, this parameter can be used to specify if they should be visible in the response body
UseLookUpViewDefinition (optional) | true | Boolean | 
IncludeDisplayViews (optional) | true | Boolean | Displays the details of the predefined arrays of columns based upon 'views' created in the Invenias Desktop application for this entity.
IncludeAvailableColumns (optional) | true | Boolean | Displays all of the column names available in the list for the entity.
IncludeCategories (optional) | true | Boolean | Returns the category lists and categories enabled for the entity.

## POST /api/v1/search/recordmanagementgroupentries

> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/recordmanagementgroupentries?request.searchTerm=London' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{}'
```

> Example Response (JSON)

```shell
{
  "Items": [
    {
      "FileAs": "London",
      "IsActive": true,
      "ItemType": "RecordManagementGroupListEntries",
      "RecordManagementGroupFileAs": "Client Locations",
      "RecordManagementGroupId": {
        "Id": "43400b86-5a99-4b84-8e7c-55701df1d37e"
      },
      "ItemId": "18a7f342-e0ca-4f76-91f0-fafc19347361",
      "Image": "INVENIAS",
      "OffLimitsStatus": "Off"
    }...
  ]...
}
```
This endpoint allows you to pass a search term and select, group, and filter Record Management Group entities to get a list of entities where the `FileAs` string matches <b>OR</b> partially matches the search term.


### HTTP Request
`https://{subdomain}.invenias.com/api/v1/search/recordmanagementgroupentries?request.searchTerm=London`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
request.searchTerm | [required] | String | Specify the desired search term for the query to be executed on the server.
Skip (optional) | 0 | Integer | Bypass a specified number of search results then return the remaining results.
Take (optional) | 0 | Integer | Specify the number of search results to return.
PageSize (optional) | 0 | Integer | Specify the number of search results to return.
PageIndex (optional) | 0 | Integer | Specify the PageIndex property to determine the index of the currently displayed page.
UsePaging (optional) | true | Boolean | 
ReturnTotalCount (optional) | true | Boolean | Displays the total number of items in the response.
ReturnTotalDatabaseItemCount (optional) | true | Boolean | Displays the total number of items in the database.
ReturnUniqueValues (optional) | true | Boolean | Returns an object containing an array of unique values queried from a given field (or values returned from an expression).
Select (optional) | [List] | Array | Specify an array of column names to be returned in the response.
Filter (optional) | [List] | Array | Filter items by column names and values.
CategoryFilter (optional) | [Record] | String | Filter items by relationally linked categories.
Sort (optional) | [List] | Array | Sort an array of items to sort by.
Group (optional) | [List] | Array | Group results together by a column in the array.
FormFactor (optional) | Any | String | Filter the items by the application used to create them 
DisplayViewId (optional) | 00000000-0000-0000-0000-000000000000 | String | Predefined arrays of columns based upon 'views' created in the Invenias Desktop application.
RequireGroupCount (optional) | true | Boolean | 
IsFirstLoad (optional) | true | Boolean | 
IncludeAdditionalValues (optional) | true | Boolean | <u>Some</u> list endpoints contain a nested array of columns named `AdditionalValues`, this parameter can be used to specify if they should be visible in the response body
UseLookUpViewDefinition (optional) | true | Boolean | 
IncludeDisplayViews (optional) | true | Boolean | Displays the details of the predefined arrays of columns based upon 'views' created in the Invenias Desktop application for this entity.
IncludeAvailableColumns (optional) | true | Boolean | Displays all of the column names available in the list for the entity.
IncludeCategories (optional) | true | Boolean | Returns the category lists and categories enabled for the entity.

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
A `400` Bad Request error indicates that the data provided in an API call is unrecognizable. The culprit is often broken or invalid JSON, perhaps a parse error. For the most part, `400` errors have already passed authentication, so the error response message will provide more specific information about the error.

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
The `401` Unauthorized status code is returned when the API request is missing authentication credentials or the credentials provided are invalid. This may happen for any of the following reasons:
<ul>
    <li>The user account username has been changed.</li>
    <li>The user account password has been reset.</li>
    <li>The user account has been disabled.</li>
    <li>The token has expired.</li>
    <li>The license has been removed from the user account.</li>
    <li>The user accounts permission group has been changed and it's no longer in the 'System Administrator' group.</li>
    <li>The tenant's database has been pushed from our production environment to another environment to be worked on (e.g. Merging databases, data cleansing, etc...) by an SI Partner or our Professional Services team and all tokens have been invalidated.</li>
    <li>The user account has been disabled by our BTO team due to exceeding 9000 429 responses within a 5 minute period. Please check your logs for 429 responses. Please note, in this event, the primary contact of the Invenias customer will be notified of this action.</li>
</ul>

## Forbidden (403) Response Code
The `403` error status code indicates the server understood the request but refuses to authorize it.

If authentication credentials were provided in the request, the server considers them insufficient to grant access. For example, a user might be trying to delete entities and the credentials being passed on the API request are for a user account without the appropriate permissions to do so.

Another reason this status code might be returned is in case the user did not request an API access token with the correct permissions.

To fix the API call for those two situations, make sure that the credentials you are using have the access level required by the endpoint, or that the access token has the correct permissions.

Please note, if you are receiving `404` or `403` response codes from our servers there's a chance the Application has expired. In this event please see [here](https://bullhorn.github.io/invenias-api-docs/#renewing-an-application) for more information on what to do. If you're unsure if your Application has expired or not please email our support team using support@invenias.com and they will be able to check for you.

## Not Found (404) Response Code
The `404` error status code indicates that the REST API can't map the client's URI to a resource but may be available in the future. ... This status code is used when our server does not wish to reveal exactly why the request has been refused, or when no other response is applicable.

Please note, if you are receiving `404` or `403` response codes from our servers there's a chance the Application has expired. In this event please see [here](https://bullhorn.github.io/invenias-api-docs/#renewing-an-application) for more information on what to do. If you're unsure if your Application has expired or not please email our support team using support@invenias.com and they will be able to check for you.

## Internal Server Error (500) Response Code
The `500` Internal Server Error server error response code indicates that the server encountered an unexpected condition that prevented it from fulfilling the request.

Please note, the database may have data management policies enabled on the entity types you're trying to work with. Please see [here](https://bullhorn.github.io/invenias-api-docs/#data-management) for more information on data management policies.

## Service Unavailable (503) Response Code

The `503` error always occurs when our servers can’t deliver the requested resources at the time the client requests them. 
Usually, this is due to a database upgrade and is accompanied by the following comment in the response body "Tenant is in Maintenance Mode - Upgrade in progress".

Other causes of `503` errors:
<ul>
    <li>The server is overloaded, meaning that is it receiving more requests than it can handle. This is why it responds with the error message. There are many reasons for an overload to occur: often an unexpected increase in traffic is the cause. Other possible reasons are malware/spam attacks as well as web applications or the content management system being incorrectly programmed.</li>
    <li>In rare cases, an incorrect DNS server configuration on the client-side (computer or router) may result in an HTTP 503 error message. The selected DNS server itself might temporarily have problems, which then results in the HTTP request showing a 'Service Unavailable' message.</li>
</ul>

In summary, `503` errors are sometimes unavoidable, it's good practice to add logic to handle them in your Application should it encounter one. This could be simple as displaying a notification to the end-users (if applicable) and polling an endpoint every 10 minutes until it receives a successful response.

<aside class="notice">
Please note, database upgrades are always scheduled outside of our customer's typical working hours and typically take under an hour to complete. If the customer has Geo-replicas of the primary database in one or more additional data centers then the database upgrade will be scheduled to run on a weekend to minimize disruption.
</aside>
