---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

includes:

search: true

code_clipboard: true
---

# Introduction
Invenias has a REST API for integrations, providing programmatic access to create, read, update and delete data in your Invenias database.

You can find the Invenias API Swagger UI documentation by navigating to https://{subdomain}.invenias.com/api, where you need to enter your subdomain in this URL.

## General Conventions
The REST API will follow specifications, conventions, and practices of HTTP and the web. This includes things like case-sensitivity of URLs, character encoding, HTTP methods, and so forth. We also observe recommended practices for REST APIs whenever appropriate.

### Entities
Invenias uses the term entity to refer to a type represented in the Invenias system. People, companies, assignments, and programmes are examples of entities. Entities capture the core concepts within the Invenias system and provide an organization for storing executive search data and applying the rules and processing that comprise the Invenias system.

### JSON 
The REST API follows the specifications and conventions of the JavaScript Object Notation (JSON) data format and any related Javascript syntax specifications. For more information, see the following articles:
<ul>
<li>[http://www.json.org](http://www.json.org)</li>
<li>[http://en.wikipedia.org/wiki/JSON](http://en.wikipedia.org/wiki/JSON)</li>
</ul>

# Authorization Flows
## Which Flow Should I Use?
The Invenias API uses the OAuth 2.0 Authorization Framework, which supports several flows (or grants). Flows are ways of retrieving an Access Token. Deciding which is suited for your use case depends mostly on your application type, but other parameters will also influence the decision (e.g. The level of trust for the client, the experience you want your users to have etc).

## OAuth 2.0 terminology

Term | Description
--------- | -----------
Resource Owner | Entity that can grant access to a protected resource. Typically, this is the end-user.
Client | Application requesting access to a protected resource on behalf of the Resource Owner.
Authorization Server | Server that authenticates the Resource Owner and issues Access Tokens after getting proper authorization.
User Agent | Agent used by the Resource Owner to interact with the Client (e.g. a browser or a native application).

## Is the Client the Resource Owner?
The first decision point is about whether the party that requires access to resources is a machine. With machine-to-machine authorization, the Client is also the Resource Owner, not requiring end-user authorization. An example is a cron job that uses an API to import information to a database. In this example, the cron job is the Client and the Resource Owner since it holds the Client ID and Client Secret and uses them to get an Access Token from the Authorization Server.

If this case matches your needs, then to learn how this flow works and how to implement it.

## Is the Client a web app executing on the server?
If the Client is a regular web app executing on a server, then the Authorization Code Flow is the flow you should use. Using this, the Client can retrieve an Access Token and, optionally, a Refresh Token. It's considered the safest choice since the Access Token is passed directly to the web server hosting the Client, without going through the user's web browser and risking exposure.

If this case matches your needs, then to learn how this flow works and how to implement it.

## Is the Client absolutely trusted with user credentials?
This decision may result in the Resource Owner Password Credentials Grant. In this flow, the end-user is asked to fill in credentials (username/password), typically using an interactive form. This information is sent to the backend and from there to Invenias. It is therefore imperative that the Client is absolutely trusted with this information.

This grant should only be used when redirect-based flows (like the Authorization Code Flow) are not possible. If this is your case, then to learn about how this flow works and how to implement it.

# Creating an Application that Supports Resource Owner Authorization Flow
## POST /api/v1/thirdpartyapplications

> Example Response (JSON)


```shell
{
  "ClientId": "your_client_id",
  "ClientSecret": "your_client_secret",
  "ExpiresOn": "2022-05-12T15:52:47.2298517+00:00",
  "IsEnabled": true,
  "Name": "Documentation Test",
  "FlowType": "ResourceOwner"
}
```
> Please note, this is the only time you'll get the client_id and client_secret - please store these securely, as you won't be able to retrieve these again.

To create a new integration, use the `POST /api/v1/thirdpartyapplications` endpoint using Swagger.

<aside class="notice">
Please note, you must have a licensed Invenias User Account and be in the 'System Administrator' permission group to perform this operation.
</aside>

Before you can use the `POST /api/v1/thirdpartyapplications` endpoint, you need to enter an `api_key` in the field at the top right-hand corner of the Swagger page. To do this, simply double click into the `api_key` field, which will generate one automatically. This will prompt you to log in if you're not already logged in. 

After Swagger returns you an `api_key`, you can then call the POST /api/v1/thirdpartyapplications endpoint to generate the `client_id` and the `client_secret`. 

From here, you will need to choose the flow type that your application requires.

<aside class="notice">
An `api_key` will be valid for 3 minutes before expiring. You can clear this field and double click it again to generate a new one to gain access for another 3 minutes.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/thirdpartyapplications`

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | How long the Application is valid for: `OneYear`, `TwoYears`, or `FiveYears`.
Name | [required] | The friendly name of your Application.
FlowType | `ResourceOwner` | The OAuth 2.0 Authorization flow.

# Creating an Application that Supports Code Authorization Flow
## POST /api/v1/thirdpartyapplications

> Example Response (JSON)


```shell
{
  "ClientId": "{clientid}",
  "ClientSecret": "{clientsecret}",
  "ExpiresOn": "2026-10-14T15:04:19.7003133+00:00",
  "IsEnabled": true,
  "Name": "MyApplication",
  "ReplyUrl": " https://invenias.com",
  "FlowType": "Code"
}
```
> Please note, this is the only time you'll get the client_id and client_secret - please store these securely, as you won't be able to retrieve these again.

To create a new integration, use the `POST /api/v1/thirdpartyapplications` endpoint using Swagger.

<aside class="notice">
Please note, you must have a licensed Invenias User Account and be in the 'System Administrator' permission group to perform this operation.
</aside>

Before you can use the `POST /api/v1/thirdpartyapplications` endpoint, you need to enter an `api_key` in the field at the top right-hand corner of the Swagger page. To do this, simply double click into the `api_key` field, which will generate one automatically. This will prompt you to log in if you're not already logged in. 

After Swagger returns you an `api_key`, you can then call the POST /api/v1/thirdpartyapplications endpoint to generate the `client_id` and the `client_secret`. 

From here, you will need to choose the flow type that your application requires.

<aside class="notice">
An api_key will be valid for 3 minutes before expiring. You can clear this field and double click it again to generate a new one to gain access for another 3 minutes.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/thirdpartyapplications`

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | How long the Application is valid for: `OneYear`, `TwoYears`, or `FiveYears`.
Name | [required] | The friendly name of your Application.
FlowType | `Code` | The OAuth 2.0 Authorization flow.
ReplyURL| [required] | The post-login URL to redirect to your Application.

# Renewing an Application
## POST /api/v1/thirdpartyapplications/{id}/renew

> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/thirdpartyapplications/363bde35-aec3-4ec7-9d33-9befdb7f0220/renew?expiration=FiveYears' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
  "ClientId": "your_client_id",
  "ClientSecret": "your_client_secret",
  "ExpiresOn": "2026-05-18T13:34:16.9289531+00:00",
  "IsEnabled": true,
  "Name": "Documentation Test",
  "FlowType": "ResourceOwner"
}
```
> Please note, this is the only time you'll get the client_secret - please store it securely.</aside>

To renew an expired application, use the POST /api/v1/thirdpartyapplications/{id}/renew endpoint using Swagger.

<aside class="notice">
Please note, you must have a licensed Invenias User Account and be in the 'System Administrator' permission group to perform this operation.
</aside>

Before you can use the `POST /api/v1/thirdpartyapplications/{id}/renew` endpoint, you need to enter an `api_key` in the field at the top right-hand corner of the Swagger page. To do this, simply double click into the `api_key` field, which will generate one automatically. This will prompt you to log in if you're not already logged in. 

After Swagger returns you an 'api_key', you can then call the `POST /api/v1/thirdpartyapplications/{id}/renew` endpoint to renew the expired application.

<aside class="notice">
An `api_key` will be valid for 3 minutes before expiring. You can clear this field and double click it again to generate a new one to gain access for another 3 minutes.
</aside>

<aside class="warning">
Please note, when renewing your application we will issue a new client_secret. Please remember to update this on your application or you cannot authenticate.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/thirdpartyapplications/{client_id}/renew?expiration=FiveYears`

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | This is the unique identifier for the expired application you wish to renew. 
expiration | [required] | The time you wish the renew the application for: `OneYear`, `TwoYears`, or `FiveYears`.

# Authentication
The Invenias API uses the OAuth 2.0 standard for authentication.

Every API integration has a `client_id` and `client_secret`, which are used when authenticating along with valid user credentials. The Invenias API supports the following grant types for authenticating:

<ul>
<li>Password (ResourceOwner)</li>
<li>Authorization Code (Code)</li>
<li>Refresh Token (Code)</li>
</ul>

Integrations using the Password grant type require a username and password to be posted as part of the authentication request instead of showing the Invenias login screen. As the user's credentials are password directly to Invenias without going via the login screen, only a user account using the Invenias Identity Provider can authenticate.

## Resource Owner Flow

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

### HTTP Request
`https://{subdomain}.invenias.com/identity/connect/token`

Parameter | Default | Description
--------- | ------- | -----------
username | [required] | Invenias user account username.
password | [required] | Invenias user account password.
client_id | [required] | This is the unique identifier for your application.
client_secret | [required] | This is a secret key known only to the application and the authorization server.
grant_type | password | The method an application uses gets an access token.
scope | openid profile api email | Scope is a mechanism in OAuth 2.0 to limit an application's access to a user's account.

You will need to use this token to authenticate when using all endpoints using the Authorization header using the format “Authorization: {type} {credentials}”, where the type is your `token_type` and the credentials are your `access_token`.

<aside class="notice">
A token is valid for 24 hours, only. It is not possible for the expiration period for tokens to be extended.

Tokens have a short expiration time ensuring that it forces applications to refresh them, giving the service a chance to revoke an application's access if needed.
</aside>

### Unauthorized requests
If a client request contains an invalid `bearer_token`, the server returns response status 401 (unauthorized). For more information on HTTP status codes, see [here](https://bullhorn.github.io/invenias-api-docs/#http-response-status-codes).

## Code Authorization Flow

The Authorization Code grant type is used by confidential and public clients to exchange an authorization code for an access token.
After the user returns to the client via the redirect URL, the application will get the authorization code from the URL and use it to request an access token.

### How it works

<img src="images\codeflow.png" alt="X-Request-Quota-Remaining" class="inline"/>
<i>Figure 1. Code Authorization Flow</i>

<ol>
	<li>The user clicks Login within the regular web application.</li>
	<li>Your application redirects the user to the Invenias Authorization Server /identity/connect/authorize endpoint).</li>
	<li>The Invenias Authorization Server redirects the user to the login and authorization prompt.</li>
	<li>The user authenticates using the default Identify Provider.</li>
	<li>The Invenias Authorization Server redirects the user back to the application with an authorization code, which is good for one use.</li>
	<li>Your application sends this code to the Invenias Authorization Server (identity/connect/token endpoint) along with the application's Client ID and Client Secret.</li>
	<li>The Invenias Authorization Server verifies the code, Client ID, and Client Secret.</li>
	<li>The Invenias Authorization Server responds with an ID Token and Access Token (and optionally, a Refresh Token).</li>
	<li>Your application can use the Access Token to call an API to access information about the user.</li>
	<li>The API responds with requested data.</li>
</ol>

### HTTP Request
`https://{subdomain}.invenias.com/identity/connect/authorize?response_type=code&client_id={clientId}&state={state} &scope=openid api profile offline_access&redirect_uri={replyurl}`

Parameter | Default | Description
--------- | ------- | -----------
response_type | `code` | Grant Type.
client_id | [required] | This is the unique identifier for your application.
state (optional) |  | The 'state' parameter preserves some state objects set by the client in the Authorization request and makes it available to the client in the response.
scope | openid api profile offline_access | Scope is a mechanism in OAuth 2.0 to limit an application's access.
redirect_uri |  | The post-login URL to redirect to your Application.

<aside class="notice">In the response there is a header named 'Location', you will need to redirect the end user to this URL via your application were they will be prompted to sign in via their chosen Identity Provider.</aside>

Integrations using the Authorization Code grant type use a redirect URL to direct users to the Invenias login screen as part of the authentication process, requiring initial user interaction to trigger the integration. As part of Invenias X, one of the additional features is Single Sign-On, as we allow customers to configure multiple Identity Providers for their users. Users can authenticate with the API using any of the enabled Identity Providers.

### Get an Access Token
Once the Invenias User has successfully signed in via an Identity Provider it will add the authentication response, including a id_token, access_token, refresh_token, refresh_token_expires_on and expires_in (if authentication was successful) to the response body of the Reply URL.

Name | Description
--------- | -----------
id_token | Also referred to as a 'token hint' the id_token can be used to end a user session and revoking the current access_token.
access_token | Access tokens are used in token-based authentication to allow an application to access an API.
expires_in | The period of time before the access_token is invalidated.
refresh_token | A refresh token allows an application to obtain a new access token without prompting the user.
refresh_token_expires_on | The date and time when the refresh token.

<aside class="notice">
A token is valid for 24 hours, only. It is not possible for the expiration period for tokens to be extended.

Tokens have a short expiration time ensuring that it forces applications to refresh them, giving the service a chance to revoke an application's access if needed.

When an access token expires, the application can use the refresh token to get a new access token. It can do this behind the scenes, and without the user's involvement so that it's a seamless process to the user. However, this will require additional code to perform this function.
</aside>

# Refresh Tokens (Code Flow Only)

> To get a new access_token using a refresh_token, use this code:

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/identity/connect/token' \
--header 'Authorization: Bearer Bearer {token}' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=refresh_token' \
--data-urlencode 'client_id={clientid}' \
--data-urlencode 'client_secret={clientsecret}' \
--data-urlencode 'refresh_token={refreshtoken}'

```

> Example Response (JSON)

```shell
{
    "id_token": "{newhint}",
    "access_token": "{newtoken}",
    "expires_in": 86400,
    "token_type": "Bearer",
    "refresh_token": "{refreshtoken}",
    "refresh_token_expires_on": "2031-10-11T13:20:01.2551196+00:00"
}
```

When access tokens expire or become invalid but the application still needs to access a protected resource, the application faces the problem of getting a new access token without forcing the user to once again grant permission. To solve this problem, OAuth 2.0 introduced an artifact called a refresh token. A refresh token allows an application to obtain a new access token without prompting the user.

## Obtaining Refresh Tokens
A refresh token can be requested by an application as part of the process of obtaining an access token. Many authorization servers implement the refresh token request mechanism defined in the OpenID Connect specification. In this case, an application must include the `offline_access` scope when initiating a request for an authorization code. After the user successfully authenticates and grants consent for the application to access the protected resource, the application will receive an authorization code that can be exchanged at the token endpoint for both an access and a refresh token.

## Using Refresh Tokens

When a new access token is needed, the application can make a POST request back to the token endpoint https://{subdomain}.invenias.com/identity/connect/token using a grant type of refresh_token (web applications need to include a client secret).
While refresh tokens are often long-lived, the authorization server can invalidate them. Some of the reasons a refresh token may no longer be valid include:
<ul>
    <li>the authorization server has revoked the refresh token.</li>
    <li>the user has revoked their consent for authorization.</li>
    <li>the refresh token has expired.</li>
    <li>the user changes their password.</li> 
    <li>the authentication policy for the resource has changed (e.g., originally the resource only used usernames and passwords, but now it requires MFA).</li>
</ul>

Because refresh tokens have the potential for a long lifetime, developers should ensure that strict storage requirements are in place to keep them from being leaked. For example, on web applications, refresh tokens should only leave the backend when being sent to the authorization server, and the backend should be secure. The client secret should be protected in a similar fashion. Mobile applications do not require a client secret, but they should still be sure to store refresh tokens somewhere only the client application can access.

# Identifying when an OAuth token has expired

There are two common approaches adopted by web developers to identify when an OAuth token has expired:

### Token Refresh Handling: Method 1

Upon receiving a valid `access_token`, `expires_in value`, `refresh_token`, etc., clients can process this by storing an expiration time as a local variable and checking it on each request. You can do this using the following steps:

<ol>
	<li>Convert expires_in to an expire time (epoch, RFC-3339/ISO-8601 datetime, etc.)</li>
	<li>Store the 'expire time'.</li>
	<li>On each resource request, check the current time against the 'expire time' and make a token refresh request before the resource request if the token has expired.</li>
</ol>

When checking the time, be sure you are (at the same time), for example, using the same timezone by converting all times to epoch or UTC timezone.

Besides receiving a new `access_token`, you may receive a new `refresh_token` with an expiration time further in the future. If you receive this, store the new refresh_token to extend the life of your session.

### Token Refresh Handling: Method 2

Another method of handling token refresh is performing a manual refresh after receiving an invalid token error. We can do this with the previous approach or by itself.

If you attempt to use an expired access_token and you get an invalid token error, perform a token refresh. Since different services can use different error codes for expired tokens, you can either keep track of the code for each service, or an easy way to refresh tokens across services is to perform a single refresh upon encountering a 401 error. 

# Rate Limiting
We use a fixed-window rate limiting strategy you can make up to `3000` api calls at any interval within a 5 minute window.

You can find out how many requests you have remaining by checking the value in the `X-Request-Quota-Remaining` response header.

<img src="images\quotaremaining.png" alt="X-Request-Quota-Remaining" class="inline"/>
<i>Figure 1. API Call Response Headers - X-Request-Quota-Remaining</i>

Once the `X-Request-Quota-Remaining reaches` 0, all succeeding requests made will return `429` errors until the current rate limit window resets. Your application should wait for the limit to become available again before making any further requests.

<aside class="warning">
Failure to add adequate mechanisms to your application instructing it to wait after receiving a 429 error may cause the user account used to authenticate the requests being disabled by Bullhorn. This would happen should it generate 9000 (or more) 429 errors within any given 5-minute period. The account will remain disabled until you have taken sufficient steps to prevent the problem from occurring again.
</aside>

# HTTP Response Status Codes
HTTP response status codes show whether it has successfully completed a specific HTTP request.
## Successful Responses

Code | Name | Description
---- | ---- | -----------
200 | OK | The request has succeeded.
201 | Created | The request has succeeded, and it has created a new resource.
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
401 | Unauthorized | Although the HTTP standard specifies “unauthorized”, semantically this response means “unauthenticated”. The client must authenticate itself to get the requested response.
403 | Forbidden | The client does not have access rights to the content; It is unauthorized, so the server is refusing to give the requested resource. Unlike 401, the client's identity is known to the server.
404 | Not Found | The server can not find the requested resource. This can also mean that the endpoint is valid, but the resource itself does not exist. Servers may also send this response instead of 403 to hide the existence of a resource from an unauthorized client. This response code is probably the most famous one because of its frequent occurrence on the web.
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
There are many list endpoints available for Invenias core entities that applications can use.

It's possible to POST a request body in a list endpoint request allowing you to define filters, sorting, grouping, column selection, pagination, and more.

<aside class="notice">
Please note, there is a row limit of <b>1000</b> rows per call when using a list endpoint.
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

If you do not specify an array of column names in the request body, the response will return the columns visible in the 'default' global display view for the entity you're requesting.

<aside class="notice">
Please note, for each item where there is 'null' data for a specified column, it will not return this column in the response to those items.
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

List endpoints have a maximum row limit of 1,000 rows per request. If you wanted to display the content in a paginated format or export all the results, you will need to use these parameters.

`PageSize`, `PageIndex` and `UsePaging` are all used to paginate the dataset:

<ul>
	<li>PageSize - how many items you want in each page (max 1,000).</li>
	<li>PageIndex - the current page you want to retrieve.</li>
</ul>


<aside class="notice">
If you wish to get all the records in a list and wish to know how many requests you will need to make, simply make a request and use the ReturnTotalDatabaseItemCount parameter. Once you know the total number of items in the database, you can determine how many requests you need to make incrementing the page index each time to get all the items in the list (e.g. TotalItemCount/PageSize = the number of requests required).
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
A group is several things that are located, gathered, or classified together.

A column in an array can leveraged to group results together.
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
The `and` and `or` operators are used to filter records based on over a single condition:

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
Data management policies are used to ensure that an organization's data and information assets are managed consistently and used properly. Invenias provides functionality allowing our customers to apply policies to any numbers of fields (within a pre-defined list) to core entities, allowing them to either make filling them in a `preferred` or `compulsory` task. 

If one or more fields within an entity are marked as `compulsory` you cannot update the entity (or create a new one) unless you populate the compulsory field(s) via the most applicable method(s).

<aside class="warning">
Failure to respect compulsory data management policies when creating or updating records will cause a failed request with a 500 HTTP code. The response may include a message like “Invenias.Model.Services.Services.Services.DataManagementService.ValidatePositionCompanyPolicy”.
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

The Invenias system has many default and customizable enumerations. When designing an integration, it may be necessary to know the values of the items in a collection for any enumeration. The GET /api/v1/settings/{key} endpoint will return the items for any enumeration name defined in the `{key}`.

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
The Invenias systems use the USPS standard for country names. When adding entries to Address fields, the value must exist within the GET/api/v1/countries list and entered entirely in uppercase (e.g. UNITED KINGDOM, FRANCE, INDIA, etc...). Failure to follow these guidelines will cause a null value in this field when creating or updating entities.
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

This endpoint can get the names of all the Lookup Lists in the database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/lookuplists/list`

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
This endpoint will get the values within a specific Lookup List.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/lookuplists/{id}/entries/list`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for a Lookup List.

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
This endpoint can create new entries on a specific Lookup List.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/lookuplists/{id}/entries/list`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for the Lookup List.
request | [required] | String | The request model used to create a new value in the Lookup List.

## PUT /api/v1/lookuplists/{id}/entries/{itemId}

# Duplicates
The Invenias REST API has two endpoints available to help flag 'People' and 'Company' type entities that <i>may</i> already exist within the database. By utilizing these endpoints, it will help maintain the integrity of the data in the database, allowing the developer to either resolve updates to an existing entity or create a new one entirely.

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
The GET /api/v1/duplicates/people endpoint flags potentially existing 'People' type entities based upon the search terms passed via the `request.personName` and `request.emailAddress` parameters.

For performance, it's <strong>strongly</strong> advised to pass the simplest search term possible via the 'request.personName'. The reason for this is that it will split the search term using the spaces as delimiters and parameters in every conceivable permutation for comparison. The more parameters that are created and comparatively references in the server-side query, the longer it will take to return a response.

### Name Components
This endpoint does not comparatively reference all the naming component fields for 'People' type entities. Please do not include salutations, suffixes, and prefixes in the 'request.personName' parameter. For a full list of the naming components leveraged by this endpoint, please see below:
<ul>
    <li>PersonFirstName</li>
    <li>PersonMiddleName</li>
    <li>PersonFamilyName</li>
    <li>PersonNickName</li>
    <li>PersonMaidenName</li>
</ul>

### Email Address
This endpoint does not comparatively reference all the email fields for 'People' type entities. It does not reference the custom email address fields. For a full list of the email address fields leveraged by this endpoint, please see below:
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
Please note, it may be optional to pass a value on both the 'request.personName' and 'request.emailAddress' parameters individually, however; you must input a value into at least one of them to make a successful request.
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
The GET /api/v1/duplicates/companies endpoint flags potentially existing 'Company' type entities based upon the search terms passed via the `request.companyName` parameter.

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
The Invenias REST API has endpoints available for most entity types, allowing you to find entities that correspond to keywords or characters specified in the search term.

<aside class="warning">
Please note, the minimum character length for the search term is 3 characters.
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
Please note, each Professional User will have both a person and User type entity. This endpoint will return information related to both the Person and user entities.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/search/users?request.searchTerm=Jane`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
request.searchTerm | [required] | String | Specify the desired search term for the query to be executed on the server.

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
This endpoint allows you to pass a search term and select, group, and filter company type entities to get a list of entities where the `FileAs` string matches <b>OR</b> partially matches the search term.


### HTTP Request
`https://{subdomain}.invenias.com/api/v1/search/companies?request.searchTerm=Old`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
request.searchTerm | [required] | String | Specify the desired search term for the query to be executed on the server.

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
This endpoint allows you to pass a search term and select, group, and filter programme type entities to get a list of entities where the `Name` string matches <b>OR</b> partially matches the search term.


### HTTP Request
`https://{subdomain}.invenias.com/api/v1/search/programmes?request.searchTerm=Network`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
request.searchTerm | [required] | String | Specify the desired search term for the query to be executed on the server.

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
This endpoint allows you to pass a search term and select, group, and filter document type entities to get a list of entities where the `AttachmentName` or `Creator` string matches <b>OR</b> partially matches the search term.


### HTTP Request
`https://{subdomain}.invenias.com/api/v1/search/documents?request.searchTerm=Jane`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
request.searchTerm | [required] | String | Specify the desired search term for the query to be executed on the server.

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

# Documents
<i>Table 1. Documents Summery</i>

Name | Description
---- | -----------
[POST /api/v1/documents/list] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-documents-list)  | Returns a list of `Document` entities in the database.
[DELETE /api/v1/documents/{id}] (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-documents-id) | Deletes a single `Document` entity per request.
[PUT /api/v1/documents/{id}] (https://bullhorn.github.io/invenias-api-docs/#put-api-v1-documents-id) | Adds or changes values to a single `Document` entity per request.
[POST /api/v1/documents/bulkDelete] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-documents-bulkdelete) | Deletes many `Document` entities per request.
[PUT /api/v1/documents/{id}/rename] (https://bullhorn.github.io/invenias-api-docs/#put-api-v1-documents-id-rename) | Renames a single `Document` entity per request.
[POST /api/v1/people/{id}/documents/list] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-people-id-documents-list) | Returns a list of `Document` entities in the database that are relationally linked to a single `Person` entity.
[POST /api/v1/people/{id}/document] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-people-id-document) | Adds a single document to Azure Blob Storage and relationally link it to a single `Person` entity.
[GET /api/v1/people/{id}/documents/{documentId}] (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-people-id-documents-documentid) | Downloads a single `Document` entity from Azure Blob Storage that is relationally linked to a single `Person` entity.
[POST /api/v1/people/{id}/documents/{documentId}/defaultCv] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-people-id-documents-documentid-defaultcv) | Flags a single `Document` entity from Azure Blob Storage that is relationally linked to a single `Person` entity as the `Default` Curriculum Vitae.
[POST /api/v1/companies/{id}/documents/list] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-id-documents-list) | Returns a list of `Document` entities in the database that are relationally linked to a single `Company` entity.
[POST /api/v1/companies/{id}/document] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-id-document) | Adds a single document to Azure Blob Storage and relationally link it to a single `Company` entity.
[GET /api/v1/companies/{id}/documents/{documentId}] (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-companies-id-documents-documentid) | Downloads a single `Document` entity from Azure Blob Storage that is relationally linked to a single `Company` entity.
[POST /api/v1/assignments/{id}/documents/list] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-assignments-id-documents-list) | Returns a list of `Document` entities in the database that are relationally linked to a single `Assignment` entity.
[POST /api/v1/assignments/{id}/document] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-assignments-id-document) | Adds a single document to Azure Blob Storage and relationally link it to a single `Assignment` entity.
[GET /api/v1/assignments/{id}/documents/{documentId}] (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-assignments-id-documents-documentid) | Downloads a single `Document` entity from Azure Blob Storage that is relationally linked to a single `Assignment` entity.
[POST /api/v1/programmes/{id}/documents/list] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-programmes-id-documents-list) | Returns a list of `Document` entities in the database that are relationally linked to a single `Programme` entity.
[POST /api/v1/programmes/{id}/document] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-programmes-id-document) | Adds a single document to Azure Blob Storage and relationally link it to a single `Programme` entity.
[GET /api/v1/programmes/{id}/documents/{documentId}] (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-programmes-id-documents-documentid) | Downloads a single `Document` entity from Azure Blob Storage that is relationally linked to a single `Programme` entity.


## POST /api/v1/documents/list
> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/documents/list' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data-raw '{
    "Select": [
        "AttachmentName",
        "DocumentTypeDisplayText",
        "ExpirationDate",
        "CreatedBy"
    ],
    "Filter": [
        "ExpirationDate",
        ">",
        "2021-04-01T00:00:00+01:00"
    ]
}'
```

> Example Response (JSON)

```shell
{
    "Items": [
        {
            "AttachmentName": "John Doe CV.docx",
            "DocumentTypeDisplayText": "Curriculum Vitae",
            "ItemId": "92582db4-5a10-47c2-94d3-59ced071d8a2",
            "ItemType": "Documents",
            "CreatedBy": "Glen Chamberlain",
            "ExpirationDate": "2021-06-01T00:00:00+01:00",
            "OffLimitsStatus": "Off"
        }...
    ]...
}
```

The POST /api/v1/documents/list endpoint will return a list of Document Type entities in the database.

Amongst other things this endpoint can be leveraged for the following purposes:
<ul>
    <li>Identifying expired documents.</li>
    <li>Searching for documents tagged as 'default' Curriculum Vitaes.</li>
    <li>Searching for documents with specific 'Document Types'.</li>
    <li>Determining the total size and number of documents in the database.</li>
</ul>

<aside class="notice">
    Please note, it's possible to POST a request body in a list endpoint request allowing you to define filters, sorting, grouping, column selection, pagination, and more.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/documents/list`

## DELETE /api/v1/documents/{id}

> Example (cURL)

```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/documents/92582db4-5a10-47c2-94d3-59ced071d8a2' \
--header 'Authorization: Bearer {token}'
```

> Please note, successful requests will return a 200 OK response code.

The DELETE /api/v1/documents/{id} endpoint is used to `permanently` delete a single Document entity per request.

<aside class="notice">
    Please note, when deleting a 'Document' entity, it will also delete any relations that exist between it and other core entities.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/documents/{id}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the document entity you wish to delete.

## PUT /api/v1/documents/{id}
> Example (cURL)

```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/documents/35351a78-40f0-463f-8912-1ebdd347644e' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "IsDefaultCv": true,
    "ExpirationDate": "2021-12-01T00:00:00.000Z"
}'
```

> Please note, successful requests will return a 200 OK response code.

The PUT /api/v1/documents/{id} endpoint can be used to add or change values to a single Document entity per request. 

<aside class="warning">
Please note, the PUT method uses the request URI to supply a changed version of the requested resource, which replaces the original version of the resource. You must include the values you wish to keep alongside those you wish to change in the request when updating the resource. Failure to do so may cause data loss.
</aside>

Amongst other things this endpoint can be leveraged for the following purposes:
<ul>
    <li>Adding an expiry date to a document.</li>
    <li>Tagging a document as a 'default' Curriculum Vitae.</li>
    <li>Tagging the document as 'Published'. </li>
</ul>


### HTTP Request
`https://{subdomain}.invenias.com/api/v1/documents/{id}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the document entity you wish to change.
request | [required] | The request model used to declare which values to change in the requested resource.

## POST /api/v1/documents/bulkDelete
> Example (cURL)

```shell
curl --location --request POST 'https://iddgc.miginvenias.com/api/v1/documents/bulkDelete' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "ItemReferences": [
        {
            "Id": "35351a78-40f0-463f-8912-1ebdd347644e"
        },
        {
            "Id": "2044b110-1805-4b15-bb1b-ec3515625f4a"
        }
    ]
}'
```

> Please note, successful requests will return a 200 OK response code.

The POST /api/v1/documents/bulkDelete endpoints allows you delete many `Document` entities in a single requests.

<aside class="notice">
Please note, when deleting a 'Document' entity, it will also delete any relations that exist between it and other core entities.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/documents/bulkDelete`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
ids | [required] | Specify the unique identifiers for the 'Document' entities you wish to delete.

## PUT /api/v1/documents/{id}/rename

> Example (cURL)

```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/documents/a548424c-2ad9-4531-ac00-41f5b4fa3332/rename?documentName=John%20Doe%20Curriculum%20Vitae' \
--header 'Authorization: Bearer {token}'
```

> Please note, successful requests will return a 200 OK response code.

The PUT /api/v1/documents/{id}/rename allows you to rename the name of a single 'Document' entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/documents/{id}/rename`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the 'Document' entity you wish to rename.
documentName | [required] | Specify the new name for the 'Document' entity.

## POST /api/v1/people/{id}/documents/list

> Example (cURL)

```shell
curl --location --request POST 'https://subdomain.invenias.com/api/v1/people/e2082622-9357-4130-aec8-c7a546906050/documents/list' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Select": [
        "AttachmentName",
        "DocumentTypeDisplayText",
        "ExpirationDate",
        "CreatedBy"
    ]
}'
```

> Example Response (JSON)

```shell
{
    "Items": [
        {
            "AttachmentName": "John Doe CV.docx",
            "DocumentTypeDisplayText": "Curriculum Vitae",
            "ItemId": "92582db4-5a10-47c2-94d3-59ced071d8a2",
            "ItemType": "Documents",
            "CreatedBy": "Glen Chamberlain",
            "ExpirationDate": "2021-06-01T00:00:00+01:00",
            "OffLimitsStatus": "Off"
        }...
    ]...
}
```

The POST /api/v1/people/{id}/documents/list endpoint will return a list of Document Type entities relationally linked to a single 'Person' entity in the database.

Amongst others, we can leverage this endpoint for the following purposes:
<ul>
    <li>Identifying expired documents.</li>
    <li>Searching for documents tagged as 'default' Curriculum Vitae's.</li>
    <li>Searching for documents with specific 'Document Types'.</li>
</ul>

<aside class="notice">
    Please note, it's possible to POST a request body in a list endpoint request allowing you to define filters, sorting, grouping, column selection, pagination, and more.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/people/{id}/documents/list`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for 'Person' entity.

## POST /api/v1/people/{id}/document

> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/people/e2082622-9357-4130-aec8-c7a546906050/document' \
--header 'Authorization: Bearer {token}' \
--form 'file=@"/C:/Users/glen.chamberlain/Documents/Jane_Doe_CV.pdf"'
```

> Example Response (JSON)

```shell
{
    "AttachmentName": "Jane_Doe_CV.pdf",
    "DocumentExtension": "application/pdf",
    "DocumentSize": 159205,
    "ItemId": "d3d7c45b-0a24-4452-b618-fc6e53833497",
    "OffLimitsStatus": "Off"
}
```

The POST /api/v1/people/{id}/document endpoint will add a document to Azure Blob Storage and relationally link it to a single 'Person' entity in the database.

### HTTP Request
`POST /api/v1/people/{id}/document`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for 'Person' entity.

## GET /api/v1/people/{id}/documents/{documentId}

> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/people/ed7c8fb4-495c-4f2a-a6a9-72b518c61da5/documents/b735c164-646a-4965-81b3-4d028989b828' \
--header 'Authorization: Bearer {token}'
```

> The response body will return the document in binary format. You can serialize binary back to its original file format by appending the documents' original file extension to the filename when saving the content of the response body.

The `GET /api/v1/people/{id}/documents/{documentId}` endpoint will retrieve the binary representation of the specified document from Azure Blob Storage that is relationally link it to a single 'Person' entity in the database. If there is over one version of the specified document, the endpoint will return the latest iteration.

<aside class="notice">
    Please note, we convert documents that are uploaded to Invenias systems to binary. This is standard industry practice. A binary file is usually very much smaller than a text file that contains an equivalent amount of data leading to faster download times.
</aside>

### HTTP Request
`GET /api/v1/people/{id}/documents/{documentId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for 'Person' entity.
documentid | [required] | String | Specify the unique identifier for 'Document' entity.

## POST /api/v1/people/{id}/documents/{documentId}/defaultCv

> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/people/ed7c8fb4-495c-4f2a-a6a9-72b518c61da5/documents/b735c164-646a-4965-81b3-4d028989b828/defaultCv' \
--header 'Authorization: Bearer {token}'
```

> A successful request will return a 204 response code "No Content".

The `POST /api/v1/people/{id}/documents/{documentId}/defaultCv` endpoint will flag the specific document as a 'Person' entities default curriculum vitae.

<aside class="notice">
    Please note, this endpoint will <strong>not</strong> parse the document and write the contents to the 'CV/Resume' tab within the 'Summary' pane in the 'Person' entities record.
</aside>

### HTTP Request
`POST /api/v1/people/{id}/documents/{documentId}/defaultCv`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for 'Person' entity.
documentid | [required] | String | Specify the unique identifier for 'Document' entity.

## POST /api/v1/companies/{id}/documents/list

> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/companies/7bc5b01f-c36a-4828-bea5-e3af7f5882e1/documents/list' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Select": [
        "AttachmentName",
        "DocumentTypeDisplayText",
        "CreatedBy",
		"DocumentExtension"
    ]
}'
```

> Example Response (JSON)

```shell
{
    "Items": [
        {
            "AttachmentName": "INVACM180821.pdf",
			"DocumentExtension": "pdf",
            "DocumentTypeDisplayText": "Invoice",
            "ItemId": "7901923e-deb6-4cbb-8742-b2587414796e",
            "ItemType": "Documents",
            "CreatedBy": "Glen Chamberlain",
            "OffLimitsStatus": "Off"
        }...
    ]...
}
```

The POST /api/v1/companies/{id}/documents/list endpoint will return a list of Document Type entities relationally linked to a single `Company` entity in the database.

<aside class="notice">
    Please note, it's possible to POST a request body in a list endpoint request allowing you to define filters, sorting, grouping, column selection, pagination, and more.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/documents/list`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for 'Company' entity.

## POST /api/v1/companies/{id}/document

> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/companies/7bc5b01f-c36a-4828-bea5-e3af7f5882e1/document' \
--header 'Authorization: Bearer {token}' \
--form 'file=@"/C:/Users/glen.chamberlain/Documents/INVACM180821.pdf"'
```

> Example Response (JSON)

```shell
{
    "AttachmentName": "INVACM180821.pdf",
    "DocumentExtension": "application/pdf",
    "DocumentSize": 189345,
    "ItemId": "7901923e-deb6-4cbb-8742-b2587414796e",
    "OffLimitsStatus": "Off"
}
```

The `POST /api/v1/companies/{id}/document` endpoint will add a document to Azure Blob Storage and relationally link it to a single 'Company' entity in the database.

### HTTP Request
`POST /api/v1/companies/{id}/document`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for 'Company' entity.

## GET /api/v1/companies/{id}/documents/{documentId}

> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/companies/7bc5b01f-c36a-4828-bea5-e3af7f5882e1/documents/7901923e-deb6-4cbb-8742-b2587414796e' \
--header 'Authorization: Bearer {token}'
```

> The response body will return the document in binary format. You can serialize binary back to its original file format by appending the documents' original file extension to the filename when saving the content of the response body.

The `GET /api/v1/companies/{id}/documents/{documentId}` endpoint will retrieve the binary representation of the specified document from Azure Blob Storage that is relationally link it to a single 'Company' entity in the database. If there is over one version of the specified document, the endpoint will return the latest iteration.

<aside class="notice">
    Please note, we convert documents that are uploaded to Invenias systems to binary. This is standard industry practice. A binary file is usually very much smaller than a text file that contains an equivalent amount of data leading to faster download times.
</aside>

### HTTP Request
`GET /api/v1/companies/{id}/documents/{documentId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for 'Company' entity.
documentid | [required] | String | Specify the unique identifier for 'Document' entity.

## POST /api/v1/assignments/{id}/documents/list

> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/assignments/7bc5b01f-c36a-4828-bea5-e3af7f5882e1/documents/list' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Select": [
        "AttachmentName",
        "DocumentTypeDisplayText",
        "CreatedBy",
		"DocumentExtension"
    ]
}'
```

> Example Response (JSON)

```shell
{
    "Items": [
        {
            "AttachmentName": "A000002_Brief.pdf",
			"DocumentExtension": "pdf",
            "DocumentTypeDisplayText": "Client Brief",
            "ItemId": "7901923e-deb6-4cbb-8742-b2587414796e",
            "ItemType": "Documents",
            "CreatedBy": "Glen Chamberlain",
            "OffLimitsStatus": "Off"
        }...
    ]...
}
```

The POST /api/v1/assignments/{id}/documents/list endpoint will return a list of Document Type entities relationally linked to a single `Assignment` entity in the database.

<aside class="notice">
    Please note, it's possible to POST a request body in a list endpoint request allowing you to define filters, sorting, grouping, column selection, pagination, and more.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/assignments/{id}/documents/list`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for 'Assignment' entity.

## POST /api/v1/assignments/{id}/document

> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/assignments/40c04603-650a-4bda-82bc-b242e0df9c7b/document' \
--header 'Authorization: Bearer {token}' \
--form 'file=@"/C:/Users/glen.chamberlain/Documents/A000002_Brief.pdf"'
```

> Example Response (JSON)

```shell
{
    "AttachmentName": "A000002_Brief.pdf",
    "DocumentExtension": "application/pdf",
    "DocumentSize": 189345,
    "ItemId": "ed25ed9b-062b-4e10-a055-6fb024fe53b2",
    "OffLimitsStatus": "Off"
}
```

The `POST /api/v1/assignments/{id}/document` endpoint will add a document to Azure Blob Storage and relationally link it to a single 'Assignment' entity in the database.

### HTTP Request
`POST /api/v1/assignments/{id}/document`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for 'Assignment' entity.

## GET /api/v1/assignments/{id}/documents/{documentId}

> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/assignments/40c04603-650a-4bda-82bc-b242e0df9c7b/documents/ed25ed9b-062b-4e10-a055-6fb024fe53b2' \
--header 'Authorization: Bearer {token}'
```

> The response body will return the document in binary format. You can serialize binary back to its original file format by appending the documents' original file extension to the filename when saving the content of the response body.

The `GET /api/v1/assignments/{id}/documents/{documentId}` endpoint will retrieve the binary representation of the specified document from Azure Blob Storage that is relationally link it to a single 'Assignment' entity in the database. If there is over one version of the specified document, the endpoint will return the latest iteration.

<aside class="notice">
    Please note, we convert documents that are uploaded to Invenias systems to binary. This is standard industry practice. A binary file is usually very much smaller than a text file that contains an equivalent amount of data leading to faster download times.
</aside>

### HTTP Request
`GET /api/v1/Assignments/{id}/documents/{documentId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for 'Assignment' entity.
documentid | [required] | String | Specify the unique identifier for 'Document' entity.

## POST /api/v1/programmes/{id}/documents/list

> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/programmes/a2adc7f7-973a-4751-99bd-c9ce7ed3c504/documents/list' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Select": [
        "AttachmentName",
        "DocumentTypeDisplayText",
        "CreatedBy",
		"DocumentExtension"
    ]
}'
```

> Example Response (JSON)

```shell
{
    "Items": [
        {
            "AttachmentName": "Talent_Mapping.xlsx",
			"DocumentExtension": "xlsx",
            "ItemId": "27569250-5c88-4d19-9d31-e02d46a403a7",
            "ItemType": "Documents",
            "CreatedBy": "Glen Chamberlain",
            "OffLimitsStatus": "Off"
        }...
    ]...
}
```

The POST /api/v1/programmes/{id}/documents/list endpoint will return a list of Document Type entities relationally linked to a single `Programme` entity in the database.

<aside class="notice">
    Please note, it's possible to POST a request body in a list endpoint request allowing you to define filters, sorting, grouping, column selection, pagination, and more.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/programmes/{id}/documents/list`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for 'Programme' entity.

## POST /api/v1/programmes/{id}/document

> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/programmes/a2adc7f7-973a-4751-99bd-c9ce7ed3c504/document' \
--header 'Authorization: Bearer {token}' \
--form 'file=@"/C:/Users/glen.chamberlain/Documents/Talent_Mapping.xlsx"'
```

> Example Response (JSON)

```shell
{
    "AttachmentName": "Talent_Mapping.xlsx",
    "DocumentExtension": "application/xlsx",
    "DocumentSize": 234123,
    "ItemId": "27569250-5c88-4d19-9d31-e02d46a403a7",
    "OffLimitsStatus": "Off"
}
```

The `POST /api/v1/programmes/{id}/document` endpoint will add a document to Azure Blob Storage and relationally link it to a single 'Programme' entity in the database.

## GET /api/v1/programmes/{id}/documents/{documentId}

> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/programmes/40c04603-650a-4bda-82bc-b242e0df9c7b/documents/27569250-5c88-4d19-9d31-e02d46a403a7' \
--header 'Authorization: Bearer {token}'
```

> The response body will return the document in binary format. You can serialize binary back to its original file format by appending the documents' original file extension to the filename when saving the content of the response body.

The `GET /api/v1/programmes/{id}/documents/{documentId}` endpoint will retrieve the binary representation of the specified document from Azure Blob Storage that is relationally link it to a single 'Programme' entity in the database. If there is over one version of the specified document, the endpoint will return the latest iteration.

<aside class="notice">
    Please note, we convert documents that are uploaded to Invenias systems to binary. This is standard industry practice. A binary file is usually very much smaller than a text file that contains an equivalent amount of data leading to faster download times.
</aside>

### HTTP Request
`GET /api/v1/programmes/{id}/documents/{documentId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for 'Programme' entity.
documentid | [required] | String | Specify the unique identifier for 'Document' entity.

# Categories
Invenias uses parent-child hierarchies to categorise core entities (e.g., People, Companies, Assignments, Programmes, etc…) within category lists. We can see a category list as a container for one or many category levels related to a class or division of people or things regarded as having shared characteristics (e.g., Industry, Position Function, Position Level, etc…).

<img src="images\categoriesparentchild.png" alt="X-Request-Quota-Remaining" class="inline"/>
<br><i>Figure 1. Invenias Professional – Category List modal.</i>
<br></br>

Please note, the number of category lists allowed in Invenias is only limited only by the resources available in the database. This means that it’s workable to have thousands of category lists; however, this is not advised as it would be unrealistic for end-users of our products to be categorizing records across this many data points.

Parent-child hierarchies have an unusual way of storing the hierarchy in the sense that they have a variable depth.

In the parent-child pattern, the hierarchy is not defined by columns in the table of the original data source. The hierarchy is based on a structure where each node of the hierarchy is related to the key to its parent node. For example, Figure 2 shows the first few rows of a parent-child hierarchy that defines an Industry structure for Assignments.

<img src="images\categoriesparentchildflat.png" alt="X-Request-Quota-Remaining" class="inline"/>
<br><i>Figure 2. Invenias Professional – Category List modal.</i>
<br></br>

The full expansion of the parent-child hierarchy in this example requires four levels. Figure 3 shows that there is one column for each level of the hierarchy. The number of columns required depends on the data, so it is possible to add additional levels to accommodate for future changes in the data.

<img src="images\categoriesparentchildpivot.png" alt="X-Request-Quota-Remaining" class="inline"/>
<br><i>Figure 3. Example of a flattened hierarchy where each level of the original parent-child hierarchy is stored in a separate column.</i>
<br></br>

<i>Table 1. Category Endpoints Summary</i>

Name | Description
---- | -----------
[POST /api/v1/categorylists]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-categorylists) | Used to create a new Category List.
[POST /api/v1/categorylists/{categoryListId}/entries]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-categorylists-categoryListId-entries) | Used to create a new category within any given Category List
[POST /api/v1/categorylists/{id}/entries/list]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-categorylists-id-entries-list) | Returns a list of all the categories that exist within a specific Category List.
[GET /api/v1/categorylists/{categoryListId}/entries/{entryId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categorylists-categoryListId-entries-entryId) | Returns the details for a specific Category List entry within the specified Category List.
[DELETE /api/v1/categorylists/{categoryListId}/entries/{entryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-categorylists-categoryListId-entries-entryId) | Used to delete a category within a specific Category List.
[GET /api/v1/categorylists/{id}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categorylists-id) | Returns the details related to a specified Category List.
[GET /api/v1/categories/jobpostings]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-jobpostings) | Returns a list of all the Category Lists enabled for `Advertisement` entities (Including Categories).
[GET /api/v1/jobpostings/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-jobpostings-id-categories) | Returns a list on Category List entries that have been relationally linked to a specific `Advertisement` entity.
[POST /api/v1/jobpostings/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-jobpostings-id-categories) | Used to relationally link a specific `Advertisement` entity with one or more categories.
[GET /api/v1/jobpostings/{id}/categories/{categoryListId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-jobpostings-id-categories-categoryListId) | This endpoint returns a list of Category List entries within a specific list where they’re relationally linked to an `Advertisement` entity.
[DELETE /api/v1/jobpostings/{id}/categories/{categoryListEntryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-jobpostings-id-categories-categoryListEntryId) | This endpoint is used to remove the relationship between an `Advertisement` and a Category List entry.
[GET /api/v1/categories/assignments]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-assignments) | Returns a list of all the Category Lists enabled for `Assignment` entities (Including Categories).
[POST /api/v1/assignments/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-assignments-id-categories) | Used to relationally link a specific `Assignment` entity with one or more categories.
[GET /api/v1/assignments/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-assignments-id-categories) | Returns a list on Category List entries that have been relationally linked to a specific `Assignment` entity.
[GET /api/v1/assignments/{assignmentId}/categories/{categoryListId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-assignments-assignmentId-categories-categoryListId) | This endpoint returns a list of Category List entries within a specific list where they’re relationally linked to an `Assignment` entity.
[DELETE /api/v1/assignments/{assignmentId}/categories/{categoryListEntryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-assignments-assignmentId-categories-categoryListEntryId) | This endpoint is used to remove the relationship between an `Assignment` and a Category List entry.
[GET /api/v1/categories/companies]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-companies) | Returns a list of all the Category Lists enabled for `Company` entities (Including Categories).
[POST /api/v1/companies/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-id-categories) | Used to relationally link a specific `Company` entity with one or more categories.
[GET /api/v1/companies/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-companies-id-categories) | Returns a list on Category List entries that have been relationally linked to a specific `Company` entity.
[GET /api/v1/companies/{companyId}/categories/{categoryListId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-companies-personId-categories-categoryListId) | This endpoint returns a list of Category List entries within a specific list where they’re relationally linked to a `Company` entity.
[DELETE /api/v1/companies/{companyId}/categories/{categoryListEntryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-companies-companyId-categories-categoryListEntryId) | This endpoint is used to remove the relationship between a `Company` and a Category List entry.
[GET /api/v1/categories/people]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-people) | Returns a list of all the Category Lists enabled for `Person` entities (Including Categories).
[POST /api/v1/people/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-people-id-categories) | Used to relationally link a specific `Person` entity with one or more categories.
[GET /api/v1/people/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-people-id-categories) | Returns a list on Category List entries that have been relationally linked to a specific `Person` entity.
[GET /api/v1/people/{personId}/categories/{categoryListId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-people-personId-categories-categoryListId) | This endpoint returns a list of Category List entries within a specific list where they’re relationally linked to a `Person` entity.
[DELETE /api/v1/people/{personId}/categories/{categoryListEntryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-people-personId-categories-categoryListEntryId) | This endpoint is used to remove the relationship between a `Person` and a Category List entry.
[GET /api/v1/categories/programmes]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-programmes) | Returns a list of all the Category Lists enabled for Programme entities (Including Categories).
[POST /api/v1/programmes/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-programmes-id-categories) | Used to relationally link a specific `Programme` entity with one or more categories.
[GET /api/v1/programmes/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-programmes-id-categories) | Returns a list on Category List entries that have been relationally linked to a specific `Programme` entity.
[GET /api/v1/programmes/{programmeId}/categories/{categoryListId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-programmes-programmeId-categories-categoryListId) | This endpoint returns a list of Category List entries within a specific list where they’re relationally linked to a `Programme` entity.
[DELETE /api/v1/programmes/{programmeId}/categories/{categoryListEntryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-programmes-programmeId-categories-categoryListEntryId) | This endpoint is used to remove the relationship between a `Programme` and a Category List entry.
[GET /api/v1/categories/assignments/{assignmentsId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-assignments-assignmentsId) | Returns a list of Category Lists & Categories relationally linked to a specific `Assignment` entity.
[GET /api/v1/categories/companies/{companyId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-companies-companyId) | Returns a list of Category Lists & Categories relationally linked to a specific `Company` entity.
[GET /api/v1/categories/people/{personId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-people-personId) | Returns a list of Category Lists & Categories relationally linked to a specific `Person` entity.
[GET /api/v1/categories/programmes/{programmesId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-programmes-programmesId) | Returns a list of Category Lists & Categories relationally linked to a specific `Programme` entity.

## POST /api/v1/categorylists
> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/categorylists' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "OptionalId": "106435bf-939a-43d7-a6b9-ea408f97bb78"
  "FileAs": "ICB",
  "OrderIndex": 0,
  "ShowDataColumns": true,
  "IsPeopleEnabled": true,
  "IsCompaniesEnabled": true,
  "IsAssignmentsEnabled": true,
  "IsAdvertisementsEnabled": true
}'

```

> Example Response (JSON)

```shell
{
    "Id": "106435bf-939a-43d7-a6b9-ea408f97bb78",
    "CategoryName": "ICB",
    "OrderIndex": 0,
    "ShowDataColumns": true
}
```

This endpoint is used to create a new Category List.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/categorylists`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
OptionalId (Optional) |  | String | Used to define the unique identifier for the new Category List.
FileAs | [required] | String | Used to define the name of the Category List.
OrderIndex (Optional) | | Integer | This specifies the ordinal position of the Category List as it will appear in Invenias systems. 
ShowDataColumns (Optional) | | Boolean | Used to enable/disable the timestamp feature in categories within the specified Category List.
IsPeopleEnabled | [required] | Boolean | Used to define if the Category List should be visible in `Person` entities within Invenias systems.
IsCompaniesEnabled | [required] | Boolean | Used to define if the Category List should be visible in `Company` entities within Invenias systems.
IsAssigmentsEnabled | [required] | Boolean | Used to define if the Category List should be visible in `Assignment` entities within Invenias systems.
IsAdvertisementsEnabled | [required] | Boolean | Used to define if the Category List should be visible in `Advertisement` entities within Invenias systems.


## POST /api/v1/categorylists/{categoryListId}/entries
> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/categorylists/e5db1242-a7c0-447a-9540-0cd5a899c96e/entries' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "OptionalId": "9af48b85-5d7b-4e92-ad1b-4c1497a5ea0a",
  "CategoryName": "0001 Oil & Gas"
}'
```

> Example Response (JSON)

```shell
{
    "Id": "9af48b85-5d7b-4e92-ad1b-4c1497a5ea0a",
    "CategoryName": "0001 Oil & Gas",
    "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
    "CategoryDisplayName": "0001 Oil & Gas"
}
```

This endpoint is used to create Categories within an existing Category List.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/categorylists/{categoryListId}/entries`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
categoryListId | [required] | String | The unique identitfier for the desired Category List.

## POST /api/v1/categorylists/{id}/entries/list
> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/categorylists/e5db1242-a7c0-447a-9540-0cd5a899c96e/entries/list' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
   "PageSize":1000,
   "PageIndex":0,
   "Select":[
      "Id",
      "ParentListEntryId",
      "FileAs"
   ]
}'
```

> Example Response (JSON)

```shell
{
    "Items": [
        {
            "FileAs": "Central African Republic",
            "ItemType": "CategoryListEntries",
            "ParentListEntryId": {
                "Id": "3e0127b5-97fd-4e4b-acd2-b463130804ad"
            },
            "ItemId": "6b79bb1f-5119-4470-99ec-01ca75932e57",
            "OffLimitsStatus": "Off"
        },
        {
            "FileAs": "Austria",
            "ItemType": "CategoryListEntries",
            "ParentListEntryId": {
                "Id": "9a157512-de16-4c5b-856c-1b091eb49129"
            },
            "ItemId": "1b4ad1c1-9f32-47da-90cc-022ee32fad95",
            "OffLimitsStatus": "Off"
        }…
}
```

This endpoint will return a list of categories within any given Category List.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/categorylists/{id}/entries/list`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | The unique identitfier for the desired Category List.

## GET /api/v1/categorylists/{categoryListId}/entries/{entryId}
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/categorylists/e5db1242-a7c0-447a-9540-0cd5a899c96e/entries/0d88ac7f-a7f2-4b46-bbcf-ad3e46fe1e8d' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Id": "0d88ac7f-a7f2-4b46-bbcf-ad3e46fe1e8d",
    "CategoryName": "Caribbean",
    "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
    "ParentListEntryId": "fd19a131-57d1-423a-b5d7-aa9b602e0d70",
    "CategoryDisplayName": "Caribbean"
}
```

This endpoint will return the details for a specific Category List entry within the specified Category List.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/categorylists/{categoryListId}/entries/{entryId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
categoryListId | [required] | String | The unique identitfier for the desired Category List.
entryId | [required] | String | The unique identifier for the desired Category List entry.

## DELETE /api/v1/categorylists/{categoryListId}/entries/{entryId}
> Example (cURL)

```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/categorylists/e5db1242-a7c0-447a-9540-0cd5a899c96e/entries/0d88ac7f-a7f2-4b46-bbcf-ad3e46fe1e8d' \
--header 'Authorization: Bearer {token}'

```

> Please note, a successful request will return a 204 response code.

This endpoint will return details related to the specified Category List.

<aside class="notice">
Please note, you can only delete a Category List entry is it’s not relationally linked to one or more core entity types (e.g., People, Assignments, Companies, Programmes, etc…). If you wish to delete a Category List entry that is relationally linked to core entity types, you will need to delete all the relations before using this endpoint to delete a Category List entry.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/categorylists/{categoryListId}/entries/{entryId}` 

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
categoryListId | [required] | String | The unique identitfier for the desired Category List.
entryId | [required] | String | The unique identifier for the desired Category List entry.

## GET /api/v1/categorylists/{id}
> Example (cURL)

```shell
Example (cURL)
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/categorylists/e5db1242-a7c0-447a-9540-0cd5a899c96e' \
--header 'Authorization: Bearer {token}'
```

This endpoint is used to delete a Category List entry within the specified Category List.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/categorylists/{id}`
 

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | The unique identitfier for the desired Category List.

## GET /api/v1/categories/jobpostings
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/categories/advertisements' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Lists": [
        {
            "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "CategoryName": "Location",
            "AssociatedColour": "#FFD9D8",
            "ShowDataColumns": true,
            "Categories": [
                {
                    "Id": "fd19a131-57d1-423a-b5d7-aa9b602e0d70",
                    "CategoryName": "AMERICAS",
                    "Children": [
                        {
                            "Id": "0d88ac7f-a7f2-4b46-bbcf-ad3e46fe1e8d",
                            "CategoryName": "Caribbean",
                            "Children": [
                                {
                                    "Id": "0880ff5b-4121-4d99-bc85-64241382669f",
                                    "CategoryName": "Anguilla",
                                    "Children": []
                                }…
                            ]
                        }
                    ]
                }
                
        }
    ]
}
```
This endpoint will return a list of all the Category Lists and their categories that are enabled on the `Advertisements` entity type.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/categories/advertisements`

## GET /api/v1/jobpostings/{id}/categories
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/jobpostings/9abed357-8915-4b90-8553-d86866af2078/categories' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Lists": [
        {
            "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "CategoryName": "Location",
            "AssociatedColour": "#FFD9D8",
            "ShowDataColumns": true,
            "Categories": [
                {
                    "Id": "62e2cb36-df4e-496f-a0dd-31d2cc2fd8d7",
                    "CategoryName": "United Kingdom",
                    "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
                    "ParentListEntryId": "9a157512-de16-4c5b-856c-1b091eb49129",
                    "CategoryDisplayName": "United Kingdom"
                }
            ]
        },
        {
            "Id": "106435bf-939a-43d7-a6b9-ea408f97bb78",
            "CategoryName": "ICB",
            "OrderIndex": 0,
            "ShowDataColumns": true,
            "Categories": []
        }
    ]
}
```

This endpoint returns a list of Category entries that have been relationally linked to a specific `Advertisement`. 

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/jobpostings/{id}/categories`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | The unique identifier for the desired `Advertisement` entity.

## POST /api/v1/jobpostings/{id}/categories
> Example (cURL) - Linking a single Category List entry to an `Advertisement` record.

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/jobpostings/9abed357-8915-4b90-8553-d86866af2078/categories' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "ItemReferences": [
    {
      "Id": "bc2f78c6-232d-42ed-9871-b3830fa0d688"
    }
  ]
}'
```

> Example (cURL) - Linking multiple Category List entries to an `Advertisement` record.

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/jobpostings/9abed357-8915-4b90-8553-d86866af2078/categories' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "ItemReferences": [
        {
            "Id": "6bcb23ec-13b5-455f-986f-727065e9f4c4"
        },
        {
            "Id": "cca86cf6-9fe8-489b-989f-96fea863822c"
        }
    ]
}'
```
> Please note, a successful request will return a 200-response code.

This endpoint is used to relationally link a specific `Advertisement` entity with one or more categories.


### HTTP Request
`https://{subdomain}.invenias.com/api/v1/jobpostings/{id}/categories`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | The unique identifier for the desired `Advertisement` entity.


## GET /api/v1/jobpostings/{id}/categories/{categoryListId}
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/jobpostings/9abed357-8915-4b90-8553-d86866af2078/categories/e5db1242-a7c0-447a-9540-0cd5a899c96e' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
Example Response (JSON)
{
    "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
    "CategoryName": "Location",
    "AssociatedColour": "#FFD9D8",
    "ShowDataColumns": true,
    "Categories": [
        {
            "Id": "6bcb23ec-13b5-455f-986f-727065e9f4c4",
            "CategoryName": "Finland",
            "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "ParentListEntryId": "9a157512-de16-4c5b-856c-1b091eb49129",
            "CategoryDisplayName": "Finland"
        },
        {
            "Id": "bc2f78c6-232d-42ed-9871-b3830fa0d688",
            "CategoryName": "France",
            "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "ParentListEntryId": "9a157512-de16-4c5b-856c-1b091eb49129",
            "CategoryDisplayName": "France"
        },
        {
            "Id": "cca86cf6-9fe8-489b-989f-96fea863822c",
            "CategoryName": "Germany",
            "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "ParentListEntryId": "9a157512-de16-4c5b-856c-1b091eb49129",
            "CategoryDisplayName": "Germany"
        },
        {
            "Id": "62e2cb36-df4e-496f-a0dd-31d2cc2fd8d7",
            "CategoryName": "United Kingdom",
            "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "ParentListEntryId": "9a157512-de16-4c5b-856c-1b091eb49129",
            "CategoryDisplayName": "United Kingdom"
        }
    ]
}
```

This endpoint returns a list of Category List entries that are relationally linked to an `Advertisement` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/jobpostings/{id}/categories/{categoryListId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | The unique identifier for the desired `Advertisement` entity.
categoryListId | [required] | String | The unique identifier for the desired Category List.

## DELETE /api/v1/jobpostings/{id}/categories/{categoryListEntryId}
> Example (cURL)

```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/jobpostings/9abed357-8915-4b90-8553-d86866af2078/categories/e5db1242-a7c0-447a-9540-0cd5a899c96e' \
--header 'Authorization: Bearer {token}'
```

> Please note, a successful request will return a 200-response code.

This endpoint is used to remove the relationship between an `Advertisement` and a Category List entry.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/jobpostings/{id}/categories/{categoryListEntryId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | The unique identifier for the desired `Advertisement` entity.
categoryListEntryId | [required] | String | The unique identifier for the desired Category List entry.

## GET /api/v1/categories/assignments
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/categories/assignments' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Lists": [
        {
            "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "CategoryName": "Location",
            "AssociatedColour": "#FFD9D8",
            "ShowDataColumns": true,
            "Categories": [
                {
                    "Id": "fd19a131-57d1-423a-b5d7-aa9b602e0d70",
                    "CategoryName": "AMERICAS",
                    "Children": [
                        {
                            "Id": "0d88ac7f-a7f2-4b46-bbcf-ad3e46fe1e8d",
                            "CategoryName": "Caribbean",
                            "Children": [
                                {
                                    "Id": "0880ff5b-4121-4d99-bc85-64241382669f",
                                    "CategoryName": "Anguilla",
                                    "Children": []
                                }…
                            ]
                        }
                    ]
                }
                
        }
    ]
}
```
This endpoint will return a list of all the Category Lists and their categories that are enabled on the `Assignments` entity type.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/categories/assignments`

## POST /api/v1/assignments/{id}/categories
> Example (cURL) - Linking a single Category List entry to an `Assignment` record.

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/assignments/9abed357-8915-4b90-8553-d86866af2078/categories' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "ItemReferences": [
    {
      "Id": "bc2f78c6-232d-42ed-9871-b3830fa0d688"
    }
  ]
}'

```

> Example (cURL) - Linking multiple Category List entries to an `Assignment` record.

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/assignments/9abed357-8915-4b90-8553-d86866af2078/categories' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "ItemReferences": [
        {
            "Id": "6bcb23ec-13b5-455f-986f-727065e9f4c4"
        },
        {
            "Id": "cca86cf6-9fe8-489b-989f-96fea863822c"
        }
    ]
}'

```
> Please note, a successful request will return a 200-response code.

This endpoint is used to relationally link a specific `Assignment` entity with one or more categories.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/assignments/{id}/categories`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | The unique identifier for the desired `Assignment` entity.

## GET /api/v1/assignments/{id}/categories
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/assignments/ca866e18-6c8f-47b0-a76a-0dd29d498e6b/categories' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Lists": [
        {
            "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "CategoryName": "Location",
            "AssociatedColour": "#FFD9D8",
            "ShowDataColumns": true,
            "Categories": [
                {
                    "Id": "bc2f78c6-232d-42ed-9871-b3830fa0d688",
                    "CategoryName": "France",
                    "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
                    "ParentListEntryId": "9a157512-de16-4c5b-856c-1b091eb49129",
                    "CategoryDisplayName": "France"
                }
            ]
        },
        {
            "Id": "32ad222b-ad9f-40c8-9499-d0bc46036723",
            "CategoryName": "Industry",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 1,
            "ShowDataColumns": false,
            "Categories": []
        },
        {
            "Id": "2bc966ed-ce7e-4c42-9da7-4cf975bbd3f4",
            "CategoryName": "Position Function",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 2,
            "ShowDataColumns": false,
            "Categories": []
        }
    ]
}
```

This endpoint returns a list of Category entries that have been relationally linked to a specific `Assignment`. 

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/assignments/{id}/categories`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | The unique identifier for the desired `Assignment` entity.

## GET /api/v1/assignments/{assignmentId}/categories/{categoryListId}
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/assignments/ca866e18-6c8f-47b0-a76a-0dd29d498e6b/categories/e5db1242-a7c0-447a-9540-0cd5a899c96e' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
    "CategoryName": "Location",
    "AssociatedColour": "#FFD9D8",
    "ShowDataColumns": true,
    "Categories": [
        {
            "Id": "bc2f78c6-232d-42ed-9871-b3830fa0d688",
            "CategoryName": "France",
            "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "ParentListEntryId": "9a157512-de16-4c5b-856c-1b091eb49129",
            "CategoryDisplayName": "France"
        }
    ]
}
```

This endpoint returns a list of Category List entries that are relationally linked to an `Assignment` entity by Category List.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/assignments/{id}/categories/{categoryListId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
assignmentId | [required] | String | The unique identifier for the desired `Assignment` entity.
categoryListId | [required] | String | The unique identifier for the desired Category List.

## DELETE /api/v1/assignments/{assignmentId}/categories/{categoryListEntryId}

> Example (cURL)

```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/assignments/ca866e18-6c8f-47b0-a76a-0dd29d498e6b/categories/e5db1242-a7c0-447a-9540-0cd5a899c96e' \
--header 'Authorization: Bearer {token}'
```

> Please note, a successful request will return a 200-response code.

This endpoint is used to remove the relationship between an `Assignment` and a Category List entry.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/assignments/{assignmentId}/categories/{categoryListEntryId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
assignmentId | [required] | String | The unique identifier for the desired `Assignment` entity.
categoryListId | [required] | String | The unique identifier for the desired Category List entry.

## GET /api/v1/categories/companies
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/categories/companies' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Lists": [
        {
            "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "CategoryName": "Location",
            "AssociatedColour": "#FFD9D8",
            "ShowDataColumns": true,
            "Categories": [
                {
                    "Id": "fd19a131-57d1-423a-b5d7-aa9b602e0d70",
                    "CategoryName": "AMERICAS",
                    "Children": [
                        {
                            "Id": "0d88ac7f-a7f2-4b46-bbcf-ad3e46fe1e8d",
                            "CategoryName": "Caribbean",
                            "Children": [
                                {
                                    "Id": "0880ff5b-4121-4d99-bc85-64241382669f",
                                    "CategoryName": "Anguilla",
                                    "Children": []
                                }…
                            ]
                        }
                    ]
                }
                
        }
    ]
}
```

This endpoint will return a list of all the Category Lists and their categories that are enabled on the `Company` entity type.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/categories/companies`

## POST /api/v1/companies/{id}/categories
> Example (cURL) - Linking a single Category List entry to a `Company` entity.

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/companies/9abed357-8915-4b90-8553-d86866af2078/categories' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "ItemReferences": [
    {
      "Id": "bc2f78c6-232d-42ed-9871-b3830fa0d688"
    }
  ]
}'

```

> Example (cURL) - Linking multiple Category List entries to a `Company` entity.

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/companies/9abed357-8915-4b90-8553-d86866af2078/categories' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "ItemReferences": [
        {
            "Id": "6bcb23ec-13b5-455f-986f-727065e9f4c4"
        },
        {
            "Id": "cca86cf6-9fe8-489b-989f-96fea863822c"
        }
    ]
}'

```
> Please note, a successful request will return a 200-response code.

This endpoint is used to relationally link a specific `Company` entity with one or more categories.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/categories`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | The unique identifier for the desired `Company` entity.

## GET /api/v1/companies/{id}/categories
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/companies/c6bd6734-fc2f-4088-bbcd-8c038e549c01/categories' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Lists": [
        {
            "Id": "32ad222b-ad9f-40c8-9499-d0bc46036723",
            "CategoryName": "Industry",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 1,
            "ShowDataColumns": false,
            "Categories": [
                {
                    "Id": "ed36bebc-bd61-462c-8407-194b60a3804d",
                    "CategoryName": "Investment Banking",
                    "CategoryListId": "32ad222b-ad9f-40c8-9499-d0bc46036723",
                    "ParentListEntryId": "7c2aaf54-2ba0-402c-814c-ac3c7195de46",
                    "CategoryDisplayName": "Investment Banking"
                }
            ]
        },
        {
            "Id": "5378859c-3346-433f-a8b1-46e74bcd88c6",
            "CategoryName": "Marketing",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 7,
            "ShowDataColumns": false,
            "Categories": []
        }…
    ]
}
```

This endpoint returns a list on Category List entries that have been relationally linked to a specific `Company` entity. 

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/categories`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | The unique identifier for the desired `Company` entity.

## GET /api/v1/companies/{companyId}/categories/{categoryListId}
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/companies/c6bd6734-fc2f-4088-bbcd-8c038e549c01/categories/e5db1242-a7c0-447a-9540-0cd5a899c96e' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
    "CategoryName": "Location",
    "AssociatedColour": "#FFD9D8",
    "ShowDataColumns": true,
    "Categories": [
        {
            "Id": "62e2cb36-df4e-496f-a0dd-31d2cc2fd8d7",
            "CategoryName": "United Kingdom",
            "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "ParentListEntryId": "9a157512-de16-4c5b-856c-1b091eb49129",
            "CategoryDisplayName": "United Kingdom"
        }
    ]
}
```

This endpoint returns a list of Category List entries that are relationally linked to an `Company` entity by Category List.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{companyId}/categories/{categoryListId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
companyId | [required] | String | The unique identifier for the desired `Company` entity.
categoryListId | [required] | String | The unique identifier for the desired Category List.

## DELETE /api/v1/companies/{companyId}/categories/{categoryListEntryId}

> Example (cURL)

```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/companies/9abed357-8915-4b90-8553-d86866af2078/categories/e5db1242-a7c0-447a-9540-0cd5a899c96e' \
--header 'Authorization: Bearer {token}'
```

> Please note, a successful request will return a 200-response code.

This endpoint is used to remove the relationship between an `Company` entity and a Category List entry.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{CompanyId}/categories/{categoryListEntryId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
companyId | [required] | String | The unique identifier for the desired `Company` entity.
categoryListId | [required] | String | The unique identifier for the desired Category List entry.

## GET /api/v1/categories/people
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/categories/people' \
--header 'Authorization: Bearer {token}'

```

> Example Response (JSON)

```shell
{
    "Lists": [
        {
            "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "CategoryName": "Location",
            "AssociatedColour": "#FFD9D8",
            "ShowDataColumns": true,
            "Categories": [
                {
                    "Id": "fd19a131-57d1-423a-b5d7-aa9b602e0d70",
                    "CategoryName": "AMERICAS",
                    "Children": [
                        {
                            "Id": "0d88ac7f-a7f2-4b46-bbcf-ad3e46fe1e8d",
                            "CategoryName": "Caribbean",
                            "Children": [
                                {
                                    "Id": "0880ff5b-4121-4d99-bc85-64241382669f",
                                    "CategoryName": "Anguilla",
                                    "Children": []
                                }…
                            ]
                        }
                    ]
                }
                
        }
    ]
}
```

This endpoint will return a list of all the Category Lists and their categories that are enabled on the `Person` entity type.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/categories/people`

## POST /api/v1/people/{id}/categories
> Example (cURL) - Linking a single Category List entry to a `Person` entity.

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/people/9abed357-8915-4b90-8553-d86866af2078/categories' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "ItemReferences": [
    {
      "Id": "bc2f78c6-232d-42ed-9871-b3830fa0d688"
    }
  ]
}'
```

> Example (cURL) - Linking multiple Category List entries to a `Person` entity.

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/people/9abed357-8915-4b90-8553-d86866af2078/categories' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "ItemReferences": [
        {
            "Id": "6bcb23ec-13b5-455f-986f-727065e9f4c4"
        },
        {
            "Id": "cca86cf6-9fe8-489b-989f-96fea863822c"
        }
    ]
}'
```
> Please note, a successful request will return a 200-response code.

This endpoint is used to relationally link a specific `Person` entity with one or more categories.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/people/{id}/categories`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | The unique identifier for the desired `Person` entity.

## GET /api/v1/people/{id}/categories
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/people/eb4cb3e4-baee-40ba-b312-847694a685bd/categories' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Lists": [
        {
            "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "CategoryName": "Location",
            "AssociatedColour": "#FFD9D8",
            "ShowDataColumns": true,
            "Categories": [
                {
                    "Id": "62e2cb36-df4e-496f-a0dd-31d2cc2fd8d7",
                    "CategoryName": "United Kingdom",
                    "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
                    "ParentListEntryId": "9a157512-de16-4c5b-856c-1b091eb49129",
                    "CategoryDisplayName": "United Kingdom"
                }
            ]
        },
        {
            "Id": "32ad222b-ad9f-40c8-9499-d0bc46036723",
            "CategoryName": "Industry",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 1,
            "ShowDataColumns": false,
            "Categories": [
                {
                    "Id": "adc6af6a-37da-49b3-a3b6-7864be2bcb71",
                    "CategoryName": "Banking",
                    "CategoryListId": "32ad222b-ad9f-40c8-9499-d0bc46036723",
                    "ParentListEntryId": "7c2aaf54-2ba0-402c-814c-ac3c7195de46",
                    "CategoryDisplayName": "Banking"
                },
                {
                    "Id": "2336f985-f26d-47b4-9037-29bad14fda5a",
                    "CategoryName": "Oil & Gas",
                    "CategoryListId": "32ad222b-ad9f-40c8-9499-d0bc46036723",
                    "ParentListEntryId": "55b6f1ba-a777-4ff3-bb63-010feb341a5f",
                    "CategoryDisplayName": "Oil & Gas"
                }
            ]
        },
        {
            "Id": "2bc966ed-ce7e-4c42-9da7-4cf975bbd3f4",
            "CategoryName": "Position Function",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 2,
            "ShowDataColumns": false,
            "Categories": [
                {
                    "Id": "f3e7b3de-9651-46f1-bc24-b9ef2981be31",
                    "CategoryName": "Executive",
                    "CategoryListId": "2bc966ed-ce7e-4c42-9da7-4cf975bbd3f4",
                    "CategoryDisplayName": "Executive"
                }
            ]
        },
        {
            "Id": "1d44368f-475e-433c-942b-109da0ab74af",
            "CategoryName": "Position Level",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 3,
            "ShowDataColumns": false,
            "Categories": [
                {
                    "Id": "6ab13256-8a17-4b61-85d6-eafd85d12707",
                    "CategoryName": "3 Senior Management",
                    "CategoryListId": "1d44368f-475e-433c-942b-109da0ab74af",
                    "CategoryDisplayName": "3 Senior Management"
                }
            ]
        },
        {
            "Id": "89826ae3-1755-4fad-aaf5-dc029e532717",
            "CategoryName": "Language",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 4,
            "ShowDataColumns": false,
            "Categories": [
                {
                    "Id": "ead32d85-1018-4a89-a1ea-ba4be6b1b550",
                    "CategoryName": "English",
                    "CategoryListId": "89826ae3-1755-4fad-aaf5-dc029e532717",
                    "SovrenCode": "en",
                    "CategoryDisplayName": "English"
                },
                {
                    "Id": "5a8b39f1-6618-4b1c-b3c5-ee44cea3992e",
                    "CategoryName": "French",
                    "CategoryListId": "89826ae3-1755-4fad-aaf5-dc029e532717",
                    "SovrenCode": "fr",
                    "CategoryDisplayName": "French"
                }
            ]
        }
    ]
}
```

This endpoint returns a list on Category List entries that have been relationally linked to a specific `Person` entity. 

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/people/{id}/categories`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | The unique identifier for the desired `Person` entity.

## GET /api/v1/people/{personId}/categories/{categoryListId}
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/people/eb4cb3e4-baee-40ba-b312-847694a685bd/categories/e5db1242-a7c0-447a-9540-0cd5a899c96e' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
    "CategoryName": "Location",
    "AssociatedColour": "#FFD9D8",
    "ShowDataColumns": true,
    "Categories": [
        {
            "Id": "62e2cb36-df4e-496f-a0dd-31d2cc2fd8d7",
            "CategoryName": "United Kingdom",
            "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "ParentListEntryId": "9a157512-de16-4c5b-856c-1b091eb49129",
            "CategoryDisplayName": "United Kingdom"
        }
    ]
}
```

This endpoint returns a list of Category List entries that are relationally linked to an `Person` entity by Category List.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/people/{personId}/categories/{categoryListId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
personId | [required] | String | The unique identifier for the desired `Person` entity.
categoryListId | [required] | String | The unique identifier for the desired Category List.

## DELETE /api/v1/people/{personId}/categories/{categoryListEntryId}

> Example (cURL)

```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/people/9abed357-8915-4b90-8553-d86866af2078/categories/e5db1242-a7c0-447a-9540-0cd5a899c96e' \
--header 'Authorization: Bearer {token}'
```

> Please note, a successful request will return a 200-response code.

This endpoint is used to remove the relationship between an `Person` entity and a Category List entry.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/people/{PeopleId}/categories/{categoryListEntryId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
personId | [required] | String | The unique identifier for the desired `Person` entity.
categoryListId | [required] | String | The unique identifier for the desired Category List entry.

## GET /api/v1/categories/programmes
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/categories/programmes' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Lists": [
        {
            "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "CategoryName": "Location",
            "AssociatedColour": "#FFD9D8",
            "ShowDataColumns": true,
            "Categories": [
                {
                    "Id": "fd19a131-57d1-423a-b5d7-aa9b602e0d70",
                    "CategoryName": "AMERICAS",
                    "Children": [
                        {
                            "Id": "0d88ac7f-a7f2-4b46-bbcf-ad3e46fe1e8d",
                            "CategoryName": "Caribbean",
                            "Children": [
                                {
                                    "Id": "0880ff5b-4121-4d99-bc85-64241382669f",
                                    "CategoryName": "Anguilla",
                                    "Children": []
                                }…
                            ]
                        }
                    ]
                }
                
        }
    ]
}
```

This endpoint will return a list of all the Category Lists and their categories that are enabled on the `Programme` entity type.

## POST /api/v1/programmes/{id}/categories
> Example (cURL) - Linking a single Category List entry to a `Person` entity.

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/programmes/9abed357-8915-4b90-8553-d86866af2078/categories' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "ItemReferences": [
    {
      "Id": "bc2f78c6-232d-42ed-9871-b3830fa0d688"
    }
  ]
}'
```

> Example (cURL) - Linking multiple Category List entries to a `Person` entity.

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/programmes/9abed357-8915-4b90-8553-d86866af2078/categories' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "ItemReferences": [
        {
            "Id": "6bcb23ec-13b5-455f-986f-727065e9f4c4"
        },
        {
            "Id": "cca86cf6-9fe8-489b-989f-96fea863822c"
        }
    ]
}'
```
> Please note, a successful request will return a 200-response code.

This endpoint is used to relationally link a specific `Programme` entity with one or more categories.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/programmes/{id}/categories`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | The unique identifier for the desired `Programme` entity.

## GET /api/v1/programmes/{id}/categories
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/programme/8f537861-7709-4ef2-bbec-68db11b19ade/categories' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Lists": [
        {
            "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "CategoryName": "Location",
            "AssociatedColour": "#FFD9D8",
            "ShowDataColumns": true,
            "Categories": [
                {
                    "Id": "bc2f78c6-232d-42ed-9871-b3830fa0d688",
                    "CategoryName": "France",
                    "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
                    "ParentListEntryId": "9a157512-de16-4c5b-856c-1b091eb49129",
                    "CategoryDisplayName": "France"
                }
            ]
        }
    ]
}
```

This endpoint returns a list on Category List entries that have been relationally linked to a specific `Programme` entity. 

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/programmes/{id}/categories`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | The unique identifier for the desired `Programme` entity.

## GET /api/v1/programmes/{programmeId}/categories/{categoryListId}
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/programmes/8f537861-7709-4ef2-bbec-68db11b19ade/categories/e5db1242-a7c0-447a-9540-0cd5a899c96e' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
    "CategoryName": "Location",
    "AssociatedColour": "#FFD9D8",
    "ShowDataColumns": true,
    "Categories": [
        {
            "Id": "62e2cb36-df4e-496f-a0dd-31d2cc2fd8d7",
            "CategoryName": "United Kingdom",
            "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "ParentListEntryId": "9a157512-de16-4c5b-856c-1b091eb49129",
            "CategoryDisplayName": "United Kingdom"
        }
    ]
}
```

This endpoint returns a list of Category List entries that are relationally linked to an `Programme` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/programmes/{id}/categories/{categoryListId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
programmeId | [required] | String | The unique identifier for the desired `Programme` entity.
categoryListId | [required] | String | The unique identifier for the desired Category List.

## DELETE /api/v1/programmes/{personId}/categories/{categoryListEntryId}
> Example (cURL)

```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/programmes/8f537861-7709-4ef2-bbec-68db11b19ade/categories/62e2cb36-df4e-496f-a0dd-31d2cc2fd8d7' \
--header 'Authorization: Bearer {token}'
```

> Please note, a successful request will return a 200-response code.

This endpoint is used to remove the relationship between an `Programme` entity and a Category List entry.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/programmes/{programmeId}/categories/{categoryListEntryId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
programmeId | [required] | String | The unique identifier for the desired `Programme` entity.
categoryListId | [required] | String | The unique identifier for the desired Category List entry.

## GET /api/v1/categories/assignments/{assignmentsId}
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/categories/assignments/{assignmentsId}' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Lists": [
        {
            "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "CategoryName": "Location",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 0,
            "ShowDataColumns": true,
            "Categories": [
                {
                    "Id": "bc2f78c6-232d-42ed-9871-b3830fa0d688",
                    "CategoryName": "France",
                    "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
                    "ParentListEntryId": "9a157512-de16-4c5b-856c-1b091eb49129",
                    "CategoryDisplayName": "France"
                }
            ]
        }
    ]
}
```

This endpoint returns a list of Category Lists & Categories relationally linked to a specific `Assignment` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/categories/assignments/{assignmentsId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
assignmentsId | [required] | String | The unique identifier for the desired `Assignment` entity.

## GET /api/v1/categories/companies/{companyId}
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/categories/companies/{companyId}' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Lists": [
        {
            "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "CategoryName": "Location",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 0,
            "ShowDataColumns": true,
            "Categories": [
                {
                    "Id": "62e2cb36-df4e-496f-a0dd-31d2cc2fd8d7",
                    "CategoryName": "United Kingdom",
                    "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
                    "ParentListEntryId": "9a157512-de16-4c5b-856c-1b091eb49129",
                    "CategoryDisplayName": "United Kingdom"
                }
            ]
        },
        {
            "Id": "32ad222b-ad9f-40c8-9499-d0bc46036723",
            "CategoryName": "Industry",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 3,
            "ShowDataColumns": false,
            "Categories": [
                {
                    "Id": "ed36bebc-bd61-462c-8407-194b60a3804d",
                    "CategoryName": "Investment Banking",
                    "CategoryListId": "32ad222b-ad9f-40c8-9499-d0bc46036723",
                    "ParentListEntryId": "7c2aaf54-2ba0-402c-814c-ac3c7195de46",
                    "CategoryDisplayName": "Investment Banking"
                }
            ]
        }
    ]
}
```

This endpoint returns a list of Category Lists & Categories relationally linked to a specific `Company` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/categories/companies/{companyId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
companyId | [required] | String | The unique identifier for the desired `Company` entity.

## GET /api/v1/categories/people/{personId}
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/categories/people/{personId}' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Lists": [
        {
            "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "CategoryName": "Location",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 0,
            "ShowDataColumns": true,
            "Categories": [
                {
                    "Id": "62e2cb36-df4e-496f-a0dd-31d2cc2fd8d7",
                    "CategoryName": "United Kingdom",
                    "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
                    "ParentListEntryId": "9a157512-de16-4c5b-856c-1b091eb49129",
                    "CategoryDisplayName": "United Kingdom"
                }
            ]
        },
        {
            "Id": "32ad222b-ad9f-40c8-9499-d0bc46036723",
            "CategoryName": "Industry",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 3,
            "ShowDataColumns": false,
            "Categories": [
                {
                    "Id": "adc6af6a-37da-49b3-a3b6-7864be2bcb71",
                    "CategoryName": "Banking",
                    "CategoryListId": "32ad222b-ad9f-40c8-9499-d0bc46036723",
                    "ParentListEntryId": "7c2aaf54-2ba0-402c-814c-ac3c7195de46",
                    "CategoryDisplayName": "Banking"
                },
                {
                    "Id": "2336f985-f26d-47b4-9037-29bad14fda5a",
                    "CategoryName": "Oil & Gas",
                    "CategoryListId": "32ad222b-ad9f-40c8-9499-d0bc46036723",
                    "ParentListEntryId": "55b6f1ba-a777-4ff3-bb63-010feb341a5f",
                    "CategoryDisplayName": "Oil & Gas"
                }
            ]
        },
        {
            "Id": "2bc966ed-ce7e-4c42-9da7-4cf975bbd3f4",
            "CategoryName": "Position Function",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 4,
            "ShowDataColumns": false,
            "Categories": [
                {
                    "Id": "f3e7b3de-9651-46f1-bc24-b9ef2981be31",
                    "CategoryName": "Executive",
                    "CategoryListId": "2bc966ed-ce7e-4c42-9da7-4cf975bbd3f4",
                    "CategoryDisplayName": "Executive"
                }
            ]
        },
        {
            "Id": "1d44368f-475e-433c-942b-109da0ab74af",
            "CategoryName": "Position Level",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 5,
            "ShowDataColumns": false,
            "Categories": [
                {
                    "Id": "6ab13256-8a17-4b61-85d6-eafd85d12707",
                    "CategoryName": "3 Senior Management",
                    "CategoryListId": "1d44368f-475e-433c-942b-109da0ab74af",
                    "CategoryDisplayName": "3 Senior Management"
                }
            ]
        },
        {
            "Id": "89826ae3-1755-4fad-aaf5-dc029e532717",
            "CategoryName": "Language",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 6,
            "ShowDataColumns": false,
            "Categories": [
                {
                    "Id": "a040349d-f437-4fe7-8c5b-f2c205a8f6a8",
                    "CategoryName": "Chinese",
                    "CategoryListId": "89826ae3-1755-4fad-aaf5-dc029e532717",
                    "SovrenCode": "zh",
                    "CategoryDisplayName": "Chinese"
                },
                {
                    "Id": "ead32d85-1018-4a89-a1ea-ba4be6b1b550",
                    "CategoryName": "English",
                    "CategoryListId": "89826ae3-1755-4fad-aaf5-dc029e532717",
                    "SovrenCode": "en",
                    "CategoryDisplayName": "English"
                },
                {
                    "Id": "5a8b39f1-6618-4b1c-b3c5-ee44cea3992e",
                    "CategoryName": "French",
                    "CategoryListId": "89826ae3-1755-4fad-aaf5-dc029e532717",
                    "SovrenCode": "fr",
                    "CategoryDisplayName": "French"
                }
            ]
        },
        {
            "Id": "8b409448-5457-4e57-a695-88aee1e78842",
            "CategoryName": "Loc Pref/Rating",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 20,
            "ShowDataColumns": false,
            "Categories": [
                {
                    "Id": "f42c79c2-4ea7-4bca-8896-14de4d318281",
                    "CategoryName": "01 A",
                    "CategoryListId": "8b409448-5457-4e57-a695-88aee1e78842",
                    "ParentListEntryId": "00a19e0f-c363-49af-b39d-bbfadae02141",
                    "CategoryDisplayName": "01 A"
                },
                {
                    "Id": "34f5b5fa-81c7-4a65-a31d-6c4a34665e6a",
                    "CategoryName": "Zurich",
                    "CategoryListId": "8b409448-5457-4e57-a695-88aee1e78842",
                    "ParentListEntryId": "775d5951-946d-45f9-925e-34455a3eddc3",
                    "CategoryDisplayName": "Zurich"
                }
            ]
        }
    ]
}
```

This endpoint returns a list of Category Lists & Categories relationally linked to a specific `Person` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/categories/people/{personId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
personId | [required] | String | The unique identifier for the desired `Person` entity.

## GET /api/v1/categories/programmes/{programmesId}
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/categories/programmes/{programmesId}' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)

```shell
{
    "Lists": [
        {
            "Id": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
            "CategoryName": "Location",
            "AssociatedColour": "#FFD9D8",
            "OrderIndex": 0,
            "ShowDataColumns": true,
            "Categories": [
                {
                    "Id": "bc2f78c6-232d-42ed-9871-b3830fa0d688",
                    "CategoryName": "France",
                    "CategoryListId": "e5db1242-a7c0-447a-9540-0cd5a899c96e",
                    "ParentListEntryId": "9a157512-de16-4c5b-856c-1b091eb49129",
                    "CategoryDisplayName": "France"
                }
            ]
        }
    ]
}
```

This endpoint returns a list of Category Lists & Categories relationally linked to a specific `Programme` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/categories/programmes/{programmesId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
programmesId | [required] | String | The unique identifier for the desired `Programme` entity.

# Parsing
Parsing
Please note, if you're considering at developing an integration that creates people in bulk using these endpoints, you must not exceed the maximum number of concurrent requests. For more information, please refer to this article on the Sovren website, [https://docs.sovren.com/Documentation/ResumeParser#batch-parsing-concurrency](https://docs.sovren.com/Documentation/ResumeParser#batch-parsing-concurrency).

Invenias uses a resume parsing tool named Sovren to provide document parsing to end-users of Invenias applications.

A resume parser is a piece of software that can read, understand, and classify all the data on a resume, just like a human can–but much faster.
It's possible for API integrations to leverage Sovren to create and/or update 'Person' entities in Invenias by parsing a source document and ingesting the structured output. However, there is no quick and easy way to create a person by parsing a document. Depending upon the type of information you wish to use, it may require validation across several areas.

You will need to determine exactly what information you wish to capture in an Invenias record to know which endpoints you're going to need to use.
When planning your integration, please consider:
<ul>
<li>What information do you want to leverage?</li>
<ul>
<li>Person Name (e.g. Prefix, Title, First name, Family name, Maiden name, Nickname etc).</li>
<li>Contact Details (e.g. Mobile phone, Home Telephone, Email adressess etc).</li> 
<li>Employment History</li>
<li>Education</li>
<li>Categories</li>
</ul>
<li>Does the person already exist?</li>
<ul>
<li>If yes, which one should need to be updated?</li> 
<li>If not, you will need to create a new Person Record.</li>
</ul>
<li>Do the Companies for their positions already exist?</li>
<ul> 
<li>If yes, which one do you want to link to the position(s) that have been found?</li> 
<li>If not, you will need to create a Company Record to link the person and position(s) to.</li>
</ul>
<li>Do the Places of Study already exist?</li>
<ul> 
<li>If yes, which one(s) do you want to link the qualification(s) found?</li> 
<li>If not, you will need to create a Company Record to link the person and qualification(s) too.</li>
</ul>
<li>Do the Categories already exist?</li> 
<ul>
<li>If yes, which one(s) do you want to link to categories?</li>
<ul><li>In which Category Lists do the categories exist?</li></ul>
<li>If not, you will need to either create the new categories in an existing category list OR create a new list and the new categories depending on the most applicable scenario</li>
</ul>
</ul>
</br>
<img src="images\parsingworkflow.png" alt="X-Request-Quota-Remaining" class="inline"/>
<br><i>Figure 1. Simple workflow example.</i>
<br></br>
<i>Table 1. Parsing Endpoints Summary</i>

Name | Description
---- | -----------
[POST /api/v2/fullparse/document] (https://bullhorn.github.io/invenias-api-docs/#post-api-v2-fullparse-document) | This endpoint converts the document, mapping the results into a structured format that an API integration can easily consume.
[GET /api/v2/fullparse/{id}] (https://bullhorn.github.io/invenias-api-docs/#get-api-v2-fullparse-id) | This endpoint will return the parsed document output from Sovren.
[POST /api/v2/fullparse/create] (https://bullhorn.github.io/invenias-api-docs/#post-api-v2-fullparse-create) | This endpoint allows the creation of a `Person` type entity.
[PUT /api/v2/fullparse/create] (https://bullhorn.github.io/invenias-api-docs/#put-api-v2-fullparse-create) | This endpoint allows you to replace a representation of the target `Person` type entity with the request payload.

## POST /api/v2/fullparse/document
> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v2/fullparse/document' \
--header 'Authorization: Bearer {token} \
--form 'file=@/C:/Users/glen.chamberlain/Downloads/JaneDoeCV.pdf'
```

> Example Response (JSON)

```shell
"cfc4a973-8c12-4fa1-9a01-39e6eb9c0843"
```
This endpoint converts the document, mapping the results into a structured format that an API integration can easily consume.

<aside class="notice">Please note, this endpoint will return a unique identifier for the parsed document. You will need to use the GET https://{subdomain}.invenias.com/api/v2/fullparse/{id} endpoint to get the results.</aside>

### HTTP Request
`POST /api/v2/fullparse/document`

## GET /api/v2/fullparse/{id}
> Example (cURL)

```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v2/fullparse/6418f819-7620-4ede-81a4-c8395519846d' \
--header 'Authorization: Bearer {token}'

```

> Example Response (JSON)

```shell
{
    "DocumentId": "cfc4a973-8c12-4fa1-9a01-39e6eb9c0843",
    "Person": {
        "NameComponents": {
            "FullName": "Jane Doe",
            "FamilyName": "Doe",
            "FirstName": "Jane"
        },
        "Position": {
            "JobTitle": "Technical Operations Engineer",
            "Description": "United Kingdom",
            "StartDate": "2020-02-01T00:00:00",
            "Company": {
                "CompanyName": "Microsoft",
                "Location": {
                    "AddressComponents": {
                        "FullAddress": "",
                        "Street": "",
                        "TownCity": "",
                        "County": "",
                        "Postcode": "",
                        "Country": ""
                    }
                }
            },
            "IsDefault": false,
            "PositionStatus": {
                "Value": 0
            }
        },
        "Location": {},
        "EmailAddresses": [
            {
                "IsPersonal": false,
                "IsBusiness": false,
                "IsVisibleAsDefault": true,
                "FieldName": "Email1Address",
                "DisplayTitle": "Email",
                "ItemValue": "jdoe123@gmail.com"
            }
        ]…
    }
```

This endpoint will return the parsed document output from Sovren.

### HTTP Request
`https://{subdomain}.invenias.com/api/v2/fullparse/{id}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | The unique identifier for the parsed document.

## POST /api/v2/fullparse/create
> Example (cURL)

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v2/fullparse/create' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Person": {
        "Owner": {
            "Id": "a9504f72-4f20-4d4a-a8d1-1a18ed571c9d",
            "DisplayTitle": "Glen Chamberlain"
        },
        "DefaultPosition": {},
        "IsCandidate": true,
        "NameComponents": {
            "FamilyName": "Jane",
            "FirstName": "Doe",
            "MiddleName": "",
            "Suffix": "",
            "Title": "",
            "Nickname": "",
            "MaidenName": ""
        },
        "HomeAddress": {
            "Street": "38 Market Place",
            "TownCity": "Reading",
            "County": "Berkshire",
            "Postcode": "RG1 2DE",
            "Country": "UNITED KINGDOM"
        },
        "EmailAddresses": [
            {
                "IsPersonal": true,
                "FieldName": "Email1Address",
                "DisplayTitle": "Email",
                "ItemValue": "jdoe123@gmail.com"
            }
        ]
    },
    "DocumentId": "cfc4a973-8c12-4fa1-9a01-39e6eb9c0843",
    "SaveParsedDocumentAsDefaultCv": true
}'

```

> Example Response (JSON)

```shell
"7621f4ed-34fd-435a-a3a1-5e5b6f2c8ecc"
```

This endpoint allows the creation of a `Person` type entity.

Please note, if you set the value for the parameter named 'SaveParsedDocumentAsDefault’ to `true` in the request body it will save the parsed source to the `Person` type entities record and flag it as the `Default` CV and write the contents to the 'CV/RESUME’ tab in the Person profile pane. This will make the text content of the document searchable when using the 'Advanced Search’ feature for `Person` type entities in Invenias Professional application.

<aside class="warning">
Please note, to create (or update) a new Person type entity, you must include the 'NameComponents’ array in the request body.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v2/fullparse/create`

## PUT /api/v2/fullparse/create

> Example (cURL)

```shell
curl --location --request PUT 'https://{subdomain}invenias.com/api/v2/fullparse/create?personId=7621f4ed-34fd-435a-a3a1-5e5b6f2c8ecc&updateAllContactItems=true' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{

   "Person":{
      "NameComponents":{
         "FamilyName":"Doe",
         "FirstName":"Jane",
         "MiddleName":"",
         "Suffix":"",
         "Title":"Ms",
         "Nickname":"",
         "MaidenName":""
      }
   },
   "DocumentId":"b6d3f059-5ce5-4c7b-92d7-a33a32bac699",
   "SaveParsedDocumentAsDefaultCv":true
}'
```

> Example Response (JSON)

```shell
"7621f4ed-34fd-435a-a3a1-5e5b6f2c8ecc"
```
This endpoint allows you to replace a representation of the target `Person` type entity with the request payload.

Please note, if you set the value for the parameter named 'SaveParsedDocumentAsDefault’ to `true` in the request body it will save the parsed source to the `Person` type entities record and flag it as the `Default` CV and write the contents to the 'CV/RESUME’ tab replacing the content in the Person profile pane. This will make the text content of the document searchable when using the 'Advanced Search’ feature for `Person` type entities in Invenias Professional application.

<aside class="warning">
Please note, to create (or update) a new Person type entity, you must include the 'NameComponents’ array in the request body or not.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v2/fullparse/create?personId=7621f4ed-34fd-435a-a3a1-5e5b6f2c8ecc&updateAllContactItems=true`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
personId | [required] | String | The unique identifier for the `Person` type entity you wish to update.
updateAllContactItems | [required] | Boolean | This should always be set to `true` regardless of whether you're updating the contact details for the `Person` type entity.


# FAQ
## Frequently asked questions
This section provides answers to common questions from both developers and customers:

### What is an API?
An application programming interface (API) is a set of routines, protocols, and tools for building software applications.

### What is the rate limit of the API?
We use a fixed-window rate limiting strategy you can make up to `3000` api calls at any interval within a 5 minute window. For more information on rate limits please see [here](https://bullhorn.github.io/invenias-api-docs/#rate-limiting).

### Will, you make changes to your API that will leave my integration unusable?
Whenever we make a change to the API we try to do so in an additive way that won't break existing integrations. However, occasionally things can change in a way that isn't backward compatible. In this event, communication will be provided.

### Can you help us with the coding?
REST is an industry standard that is not dependent on any programming language or technology apart from HTTP. Our support team may not help you with your coding problems as they are not aware of all the ways to implement a web service client and have limited knowledge about your platform architecture or technology stack. Therefore, Invenias may not guarantee and support your code.

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

Please review every single piece of text in the request, ensuring that there are no typos in the endpoint, headers (name and values), and body. If you copied and pasted any part of your API request, pay extra attention that they don't include any mistakes or random characters that could cause an issue.

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
    <li>The user accounts permission group has been changed, and it's no longer in the 'System Administrator' group.</li>
    <li>The tenant's database has been pushed from our production environment to another environment to be worked on (e.g. Merging databases, data cleansing, etc...) by an SI Partner or our Professional Services team and all tokens have been invalidated.</li>
    <li>The user account has been disabled by our BTO team due to exceeding 9000 429 responses within a 5 minute period. Please check your logs for 429 responses. Please note, in this event, the primary contact of the Invenias customer will be notified of this action.</li>
</ul>

## Forbidden (403) Response Code
The `403` error status code shows the server understood the request but refuses to authorize it.

If authentication credentials were provided in the request, the server considers them insufficient to grant access. For example, a user might try to delete entities and the credentials being passed on the API request are for a user account without the permissions to do so.

Another reason this status code might be returned is in case the user did not request an API access token with the correct permissions.

To fix the API call for those two situations, make sure that the credentials you are using have the access level required by the endpoint, or that the access token has the correct permissions.

Please note, if you are receiving `404` or `403` response codes from our servers, there's a chance the Application has expired. In this event please see [here](https://bullhorn.github.io/invenias-api-docs/#renewing-an-application) for more information on what to do. If you're unsure if your application has expired or not please email our support team using support@invenias.com and they will check for you.

## Not Found (404) Response Code
The `404` error status code indicates that the REST API can't map the client's URI to a resource, but may be available in the future. ... This status code is used when our server does not wish to reveal exactly why the request has been refused, or when no other response applies.

Please note, if you are receiving `404` or `403` response codes from our servers, there's a chance the Application has expired. In this event please see [here](https://bullhorn.github.io/invenias-api-docs/#renewing-an-application) for more information on what to do. If you're unsure if your application has expired or not, please email our support team using support@invenias.com and they will check for you.

## Internal Server Error (500) Response Code
The `500` Internal Server Error server error response code shows that the server encountered an unexpected condition that prevented it from fulfilling the request.

Please note, the database may have data management policies enabled on the entity types you're trying to work with. Please see [here](https://bullhorn.github.io/invenias-api-docs/#data-management) for more information on data management policies.

## Service Unavailable (503) Response Code

The `503` error always occurs when our servers can't deliver the requested resources at the time the client requests them. 
Usually, this is due to a database upgrade and is accompanied by the following comment in the response body "Tenant is in Maintenance Mode - Upgrade in progress".

Other causes of `503` errors:
<ul>
    <li>The server is overloaded, meaning that is it receiving more requests than it can handle. Therefore, it responds with the error message. There are many reasons for an overload to occur: often an unexpected increase in traffic is the cause. Other possible reasons are malware/spam attacks and web applications, or the content management system being incorrectly programmed.</li>
    <li>In rare cases, an incorrect DNS server configuration on the client-side (computer or router) may cause an HTTP 503 error message. The selected DNS server itself might temporarily have problems, which then results in the HTTP request showing a 'Service Unavailable' message.</li>
</ul>

In summary, `503` errors are sometimes unavoidable, it's good practice to add logic to handle them in your application should it encounter one. This could be simple as displaying a notification to the end-users (if applicable) and polling an endpoint every 10 minutes until it receives a successful response.

<aside class="notice">
Please note, database upgrades run outside of our customer's typical working hours and typically take under an hour to complete. If the customer has Geo-replicas of the primary database in one or more additional data centers, then the database upgrade will run on a weekend to minimize disruption.
</aside>
