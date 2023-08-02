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

### Subdomains
To ensure optimal performance and user experience, third-party applications are advised to make requests via a dedicated subdomain, distinct from the one used by Invenias applications. This best practice effectively prevents Invenias users from encountering rate-limiting issues, leading to a smoother and more seamless user experience.

<aside class="notice">
If you are uncertain about whether the customers you are working with have an existing dedicated Invenias subdomain for third-party application usage, please feel free to reach out to us for clarification and guidance. We are here to assist you in ensuring a successful and hassle-free integration process.
</aside>

# Authorization Flows
## Which Flow Should I Use?
The Invenias API leverages the OAuth 2.0 Authorization Framework, providing support for various flows (or grants) that facilitate the retrieval of an Access Token. Selecting the most suitable flow for your specific use case primarily depends on your application type, but other factors, such as the level of trust for the client and the desired user experience, also play a significant role in the decision-making process. By carefully considering these parameters, you can opt for the OAuth flow that best aligns with your project's requirements, ensuring a secure and seamless user experience.

## OAuth 2.0 terminology
Term | Description
--------- | -----------
Resource Owner | Entity that can grant access to a protected resource. Typically, this is the end-user.
Client | Application requesting access to a protected resource on behalf of the Resource Owner.
Authorization Server | Server that authenticates the Resource Owner and issues Access Tokens after getting proper authorization.
User Agent | Agent used by the Resource Owner to interact with the Client (e.g. a browser or a native application).

## Is the Client the Resource Owner?
The first crucial consideration revolves around whether the entity requiring resource access is a machine. In the context of machine-to-machine authorization, the Client itself serves as the Resource Owner, thereby eliminating the need for end-user authorization. A classic illustration of this scenario is a cron job employing an API to import data into a database. In this instance, the cron job acts as both the Client and the Resource Owner, possessing the Client ID and Client Secret, and utilizing them to obtain an Access Token from the Authorization Server.

If this scenario aligns with your requirements, we encourage you to explore the workings of this flow and its implementation steps. Delve into the details to understand how this seamless machine-to-machine authorization can elevate your project's efficiency and reliability.

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

To set up a new integration, launch a `POST /api/v1/thirdpartyapplications` request through the Swagger interface. It's noteworthy that multiple third-party applications can be registered. For each additional application needed, just repeat the registration procedure.

<aside class="notice">
Please note, you must have a licensed Invenias User Account and be in the 'System Administrator' permission group to perform this operation.
</aside>

Before you can use the `POST /api/v1/thirdpartyapplications` endpoint, you need to enter an `api_key` in the field at the top right-hand corner of the Swagger page. To do this, simply double click into the `api_key` field, which will generate one automatically. This will prompt you to log in if you're not already logged in. 

After Swagger returns you an `api_key`, you can then call the POST /api/v1/thirdpartyapplications endpoint to generate the `client_id` and the `client_secret`. 

Subsequently, you'll need to select the flow type that best suits your application's requirements.

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

To set up a new integration, launch a `POST /api/v1/thirdpartyapplications` request through the Swagger interface. It's noteworthy that multiple third-party applications can be registered. For each additional application needed, just repeat the registration procedure.

<aside class="notice">
<<<<<<< Updated upstream
Please note, you must have a licensed Invenias User Account and be in the 'System Administrator' permission group to perform this operation.
=======
Kindly observe that you must possess a licensed Invenias User Account and be a member of the 'System Administrator' permission group to execute this operation.
>>>>>>> Stashed changes
</aside>

Prior to utilizing the `POST /api/v1/thirdpartyapplications` endpoint, it is necessary to input an `api_key` into the designated field at the upper right-hand corner of the Swagger page. Achieve this by double-clicking the `api_key` field, which will consequently generate the key. If you're not already logged in, this action will trigger a login prompt.

Upon receipt of an `api_key` from Swagger, you're eligible to call the POST /api/v1/thirdpartyapplications endpoint to generate both the `client_id` and `client_secret`.

Subsequently, you'll need to select the flow type that best suits your application's requirements.

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

In situations where access tokens expire or become invalid, and the application requires continued access to protected resources without re-prompting the user for permission, OAuth 2.0 offers a solution through a mechanism known as a refresh token. This invaluable artifact enables applications to obtain a new access token seamlessly, avoiding the need for user intervention or re-authorization. By employing refresh tokens, OAuth 2.0 enhances the user experience while maintaining security and efficient access to protected resources.

## Obtaining Refresh Tokens
A refresh token can be obtained by an application during the process of acquiring an access token. The refresh token request mechanism is commonly implemented by various authorization servers, following the guidelines outlined in the OpenID Connect specification. To initiate this process, the application must include the `offline_access` scope when requesting an authorization code.

Once the user successfully authenticates and grants consent for the application to access the protected resource, the application will receive an authorization code. This code can then be exchanged at the token endpoint to obtain both an access token and a refresh token. By following this procedure, applications can efficiently manage access to protected resources and ensure continuous interactions with the authorization server.

## Using Refresh Tokens

To obtain a new access token when needed, the application can make a POST request to the token endpoint: https://{subdomain}.invenias.com/identity/connect/token. The grant type used for this request is refresh_token (web applications must include a client secret).

Refresh tokens, while typically long-lived, can be invalidated by the authorization server for various reasons, including:
<ul>
    <li>The authorization server has revoked the refresh token.</li>
    <li>The user has revoked their consent for authorization.</li>
    <li>The refresh token has expired.</li>
    <li>The user changes their password.</li> 
    <li>The authentication policy for the resource has changed (e.g., originally the resource only used usernames and passwords, but now it requires MFA).</li>
</ul>

Due to the potential long lifetime of refresh tokens, developers must ensure strict storage requirements are in place to prevent leaks. For web applications, it is crucial to ensure that refresh tokens only leave the backend when sent to the authorization server, and the backend itself should be highly secure. Similarly, the client secret should be protected with equal diligence. For mobile applications, a client secret is not required, but it is essential to store refresh tokens in a location accessible only to the client application. By implementing these measures, developers can maintain the security and integrity of the refresh token system while ensuring smooth and secure interactions with the authorization server.

# Identifying when an OAuth token has expired

There are two common approaches adopted by web developers to identify when an OAuth token has expired:

### Token Refresh Handling: Method 1

Upon receiving a valid `access_token`, `expires_in` value, `refresh_token`, etc., clients can effectively manage token expiration by following these steps:

<ol>
	<li>Convert the `expires_in` value to an expiration time format such as epoch, RFC-3339, or ISO-8601 datetime.</li>
	<li>Store the obtained 'expire time' locally as a variable.</li>
	<li>For each resource request, compare the current time against the stored 'expire time'. If the token has expired, initiate a token refresh request before proceeding with the resource request.</li>
</ol>

When performing time comparisons, ensure consistency in the timezone used, such as converting all times to epoch or UTC timezone.

Additionally, in certain scenarios, besides receiving a new `access_token`, you may also obtain a new `refresh_token` with a further extended expiration time. In such cases, it is essential to store the new `refresh_token` to prolong the session's validity and ensure continuous access to the resources. By diligently managing token expiration and refresh, clients can ensure smooth and secure interactions with the API.

### Token Refresh Handling: Method 2

Handling token refresh can be achieved through a manual refresh method when encountering an invalid token error. This approach can be used independently or combined with the previous method.

If you receive an invalid token error while attempting to use an expired access_token, trigger a token refresh. Since different services may use varying error codes for expired tokens, you have two options: either keep track of the code for each service or opt for a more straightforward approach by performing a single refresh whenever a 401 error is encountered across services. This streamlined approach ensures that token refresh is efficiently managed, enabling smooth and uninterrupted access to the required services.

# Rate Limiting
We employ a fixed-window rate limiting strategy that allows you to execute up to `3000` API calls within a 5-minute window.

To keep track of your remaining requests, refer to the `X-Request-Quota-Remaining` response header.

<img src="images\quotaremaining.png" alt="X-Request-Quota-Remaining" class="inline"/>
<i>Figure 1. API Call Response Headers - X-Request-Quota-Remaining</i>

Once the `X-Request-Quota-Remaining` value reaches 0, any subsequent requests will result in `429` errors until the current rate limit window resets. It is essential for your application to wait until the limit becomes available again before making further requests.

<aside class="warning">
Failure to implement adequate mechanisms in your application to handle 429 errors and enforce a waiting period may lead to the user account used for authentication being disabled by Bullhorn. This can happen if the account generates 9000 (or more) 429 errors within any 5-minute period. The account will remain disabled until appropriate measures are taken to prevent such occurrences.
</aside>

# HTTP Supported Request Methods 
<ol>
  <li><strong>GET:</strong> This HTTP method is used to retrieve information from the given server using a given URI. Requests using GET should only retrieve data and should have no other effect on the data.</li>
  <li><strong>POST:</strong> A POST request is used to send data to the server, for example, customer information, file upload, etc. using HTML forms. The submitted data is stored in the request body of the HTTP request.</li>
  <li><strong>PUT:</strong> PUT is used to send data to a server to create or update a resource. The difference between POST and PUT is that PUT requests are idempotent. That is, calling the same PUT request multiple times will always produce the same result. In contrast, calling a POST request repeatedly may have side effects of creating the same resource multiple times.</li>
  <li><strong>PATCH:</strong> It is used for modify capabilities. The PATCH HTTP method allows partial update on a resource. For example, if you only need to update one field of the resource, you may use the PATCH method. The HTTP PATCH request only needs to contain the changes to the resource, not the complete resource. This resembles PUT, but the body contains a set of instructions describing how a resource currently residing on the server should be modified to produce a new version.</li>
</ol>
<p>Each method serves a different purpose in the RESTful API development, and understanding them can help ensure the correct operations are used when interacting with any API service.</p>


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

<aside class="warning">
Please note, the ‘IncludeDisplayViews’ property should only be ‘true’ if you need a list of all the ‘Display Views’ and their properties used in the Invenias applications. Including ‘Display Views’ adds additional SQL server load, increasing the request duration and returning larger payloads to the client machine. If you require information about ‘Display Views’ for any entity list, you should only get them on the first request, ensuring that in subsequent requests the ‘IncludeDisplayViews’ property is ‘false’.
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
IsFirstLoad (optional) | true | Boolean | Used to identify the first request in a series.
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

# Backups
<strong>Prerequisites</strong>
<ul>
<li>The request must me made by a licensed, active Invenias user with 'System Administrator' permissions.</li>
<li>If using Swagger, before calling the endpoint you will need to double click the 'api_key' box in the top right-hand corner of the Swagger page. The key will expire within 3 minutes, if it does please repeat the process to get another key.</li>
</ul>

<strong>Considerations</strong>
<ul>
<li>The user who makes the backup request is the only person who can access the backup via Azure Storage Explorer.</li>
<li>There are cost implications when requesting over one backup. Please contact your Invenias account manager for details.</li>
</ul>

## Requesting a Backup
> To request a backup, use this code:

```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/backuprequests' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "Reason": "API Integration Project: Our web developer would like to analyse the database schema and objects for a data warehousing tool we are building."
}'
```
> Please note, successful requests will return a 204 No Content response code.

To initiate a backup request, simply call the endpoint https://{subdomain}.invenias.com/api/v1/backuprequests and provide a clear explanation of why you need the backup.

Once your request is submitted, our finance department will carefully review it and proceed with the necessary approvals. Please be aware that this process may take several working days to complete.

Rest assured, we prioritize the security and accuracy of your data, and our team will ensure that your backup request is handled diligently and with utmost care. Thank you for entrusting us with your data management needs.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/backuprequests`

## Accessing a Backup
After receiving confirmation that your request has been approved and the backup is ready, you can proceed with obtaining the required SAS token to access the storage account where the backup is hosted. To do this, utilize the same user account that was used to make the initial request.

To acquire the SAS token, make a GET request to the following endpoint using the appropriate method. Once obtained, you can seamlessly access the storage account using Azure Storage Explorer or any other Azure Storage SDK, allowing you to efficiently manage and retrieve the backup data as needed.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/backuprequests`

# Data Management
Data management policies play a critical role in maintaining the consistency and proper utilization of an organization's data and information assets. With Invenias, our customers can apply policies to a predefined list of fields across core entities, providing the flexibility to designate them as `preferred` or `compulsory` for data entry.

When a field within an entity is marked as `compulsory`, it becomes mandatory to populate the field before updating the entity or creating a new one. This requirement ensures that vital information is captured accurately, promoting data accuracy and completeness.

By incorporating data management policies, Invenias empowers organizations to enforce data standards, minimize errors, and achieve better data quality and integrity throughout their operations.

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
A Lookup List serves as a collection of values that are either utilized or recommended for use in a particular field. Essentially, it can be formed by values entered by users into the field or be pre-loaded with predefined values. This versatile feature allows for dynamic and user-driven creation of the list, as well as the ability to curate the list in advance, ensuring a smooth and user-friendly experience in populating fields with appropriate and relevant values.


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
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/lookuplists/{id}/entries/{itemId}' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Name": "Panel Interview"
}'
```

> Example Response (JSON)
```shell
{
    "Items": [
        {
            "IsActive": false,
            "ItemType": "LookupListEntries",
            "LookupListId": {
                "Id": "a1eef6c2-0f04-42db-8b93-142fb1a458aa"
            },
            "Name": "Panel Interview",
            "OrderIndex": 0,
            "ItemId": "8be50372-ad31-4949-96c9-0d95e67f7986",
            "OffLimitsStatus": "Off"
        }
    ],
}
```

This endpoint can be used to change values to a single Lookup List entry per request.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/lookuplists/{id}/entries/{itemId}`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for the Lookup List.
itemId | [required] | String | Specify the unique identifier for the Lookup List entry you wish to change.

# Duplicates
The Invenias REST API provides two essential endpoints to facilitate the flagging of 'People' and 'Company' type entities that <i>may</i> already exist within the database. By leveraging these endpoints, developers can effectively preserve the data's integrity, empowering them to handle updates to existing entities or create entirely new ones based on the context. This feature ensures seamless data management and reduces the risk of duplicate records, enhancing the overall reliability and accuracy of the database.

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
        "ItemId": "{id}",
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
        "ItemId": "{id}",
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
The Invenias REST API offers a wide range of endpoints covering most entity types, empowering you to search for entities that align with specific keywords or characters provided in the search term. This extensive functionality enables efficient and precise retrieval of relevant information, making it easier for you to work with diverse datasets and streamline your application's data retrieval process.

<aside class="warning">
Please note, the minimum character length for the search term is 3 characters.
</aside>

<aside class="notice">
The 'Quick Search' and 'Search' endpoints differ significantly in functionality. While the 'Quick Search' endpoints provide a straightforward search capability, the 'Search' endpoints offer advanced features such as column selection, filtering, and result grouping. With the 'Search' endpoints, you gain greater flexibility in tailoring your queries and obtaining more targeted and structured search results. On the other hand, the 'Quick Search' endpoints are more suitable for simple and rapid searches without additional customization options.
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

# Journal
In Invenias, journal items play a crucial role in recording various activities, including Telephone calls, Emails, and Interviews. These actions are efficiently stored in the Invenias Global Journal and can be linked to one or more core entities, such as People, Companies, Assignments, and Programmes. This comprehensive system allows for seamless tracking and organization of essential interactions, ensuring a holistic view of each entity's history and associated activities.

<aside class="notice">
Please be aware that several endpoints used for creating journal item types may not support sending emails, documents, or meeting invitations via end-users' email accounts upon the creation of a new resource. It's important to understand that functionalities such as Email, Interview, Task, Sending CVs, and documents are primarily intended for historical purposes.

These endpoints are designed to capture and log relevant data in the database without triggering actions like sending emails or meeting invitations. Their main focus is on maintaining historical records and facilitating data tracking, rather than facilitating real-time interactions with end-users' email accounts or scheduling systems.
</aside>

<p><i>Table 1. Journal Endpoint Summary</i></p>
<table>
    <thead>
        <tr>
            <th>Journal Item Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan=1>Global Journal List</td>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#post-api-v1-journal-list">POST /api/v1/journal/list</a> </td>
            <td>Used to retrieve a list of global 'Journal' entities in the database.</td>
        </tr>
        <tr>
            <td rowspan=6>Appointment / Meeting</td>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#post-api-v1-appointments">POST /api/v1/appointments</a> </td>
            <td>Used to create an 'Appointment' journal entity in the database.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#post-api-v1-appointments-list">POST /api/v1/appointments/list</a> </td>
            <td>Get a lists of 'Appointment' journal entities in the database.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#get-api-v1-appointments-upcoming">GET /api/v1/appointments/upcoming</a> </td>
            <td>Get the next five upcoming 'Appointment' journal entities scheduled to take place in the future.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#get-api-v1-appointments-id">GET /api/v1/appointments/{id}</a></td>
            <td>Get details for any given 'Appointment' journal entity in the database.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#put-api-v1-appointments-id">PUT /api/v1/appointments/{id}</a></td>
            <td>Allows you to replace a representation of the target 'Appointment' journal entity with the request payload.</td>
        </tr>
               <tr>
        <td><a ref="https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-appointments-id">DELETE /api/v1/appointments/{id}</a></td>
        <td>Allows you to permanently delete any given 'Appointment' entity.</td>
        </tr>
        <tr>
            <td rowspan=4>Sent Document</td>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#post-api-v1-sentcurriculumvitaes">POST /api/v1/sentdocuments</a> </td>
            <td>Used to create an 'Sent Document' journal entity in the database.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#get-api-v1-sentcurriculumvitaes-id">GET /api/v1/sentdocuments/{id}</a> </td>
            <td>Get details for any given 'Sent Document' journal entity in the database.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#put-api-v1-sentcurriculumvitaes-id">PUT /api/v1/sentdocuments/{id}</a> </td>
            <td>Allows you to replace a representation of the target 'Sent Document' journal entity with the request payload.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-sentcurriculumvitaes-id">DELETE /api/v1/sentdocuments/{id}</a></td>
            <td>Allows you to permanently delete any given 'Sent Document' journal entity.</td>
        </tr>
        <tr>
            <td rowspan=4>Email</td>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#post-api-v1-emails">POST /api/v1/emails</a> </td>
            <td>Used to create an 'Email' journal entity in the database.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#get-api-v1-emails-id">GET /api/v1/emails/{id}</a> </td>
            <td>Get details for any given 'Email' journal entity in the database.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#put-api-v1-emails-id">PUT /api/v1/emails/{id}</a> </td>
            <td>Allows you to replace a representation of the target 'Email' journal entity with the request payload.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-emails-id">DELETE /api/v1/emails/{id}</a></td>
            <td>Allows you to permanently delete any given 'Email' journal entity.</td>
        </tr>
        <tr>
            <td rowspan=8>Feedback</td>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#post-api-v1-feedback">POST /api/v1/feedback</a> </td>
            <td>Used to create an 'Feedback' entity for any given 'Person' journal entity in the database.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#post-api-v1-feedback-id-comments">POST /api/v1/feedback/{id}/comments</a> </td>
            <td>Used to add a comment from 'Person' entity to an existing 'Feedback' journal entity.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#get-api-v1-feedback-id">GET /api/v1/feedback/{id}</a> </td>
            <td>Get details for any given 'Feedback' journal entity in the database.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#get-api-v1-feedback-id-comments-commentId">GET /api/v1/feedback/{id}/comments/{commentId}</a></td>
            <td>Get a list of 'Comment' entities that are relationally linked to a given 'Feedback' journal entity in the database.</td>
        </tr>
                <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#put-api-v1-feedback-id">PUT /api/v1/feedback/{id}</a></td>
            <td>Allows you to replace a representation of the target 'Feedback' journal entity with the request payload.</td>
        </tr>
                <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#put-api-v1-feedback-id-comments-commentId">PUT /api/v1/feedback/{id}/comments/{commentId}</a></td>
            <td>Allows you to replace a representation of the target 'Comment' entity that is relationally linked to any given 'Feedback' journal entity in the database with the request payload.</td>
        </tr>
        </tr>
                <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-feedback-id">DELETE /api/v1/feedback/{id}</a></td>
            <td>Allows you to permanently delete any given 'Feedback' journal entity.</td>
        </tr>
        </tr>
                <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-feedback-id-comments-commentId">DELETE /api/v1/feedback/{id}</a></td>
            <td>Allows you to permanently delete any given 'Comment' entity that is relationally linked to a given 'Feedback' journal entity in the database.</td>
        </tr>
        <tr>
            <td rowspan=4>Interview</td>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#post-api-v1-interviews">POST /api/v1/interviews</a> </td>
            <td>Used to create an 'Interview' journal entity.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#get-api-v1-interviews-id">GET /api/v1/interviews/{id}</a> </td>
            <td>Get details for any given 'Interview' journal entity in the database.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#put-api-v1-interviews-id">PUT /api/v1/interviews/{id}</a></td>
            <td>Allows you to replace a representation of the target 'Interview' journal entity with the request payload.</td>
        </tr>
                <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-interviews-id">DELETE /api/v1/interviews/{id}</a></td>
            <td>Allows you to permanently delete any given 'Interview' journal entity.</td>
        </tr>
        <tr>
            <td rowspan=4>Note</td>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#post-api-v1-notes">POST /api/v1/notes</a> </td>
            <td>Used to create an 'Note' journal entity.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#get-api-v1-notes-id">GET /api/v1/notes/{id}</a> </td>
            <td>Get details for any given 'Note' journal entity in the database.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#put-api-v1-notes-id">PUT /api/v1/notes/{id}</a></td>
            <td>Allows you to replace a representation of the target 'Note' journal entity with the request payload.</td>
        </tr>
                <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-notes-id">DELETE /api/v1/notes/{id}</a></td>
            <td>Allows you to permanently delete any given 'Note' journal entity.</td>
        </tr>
        <tr>
            <td rowspan=4>Phone Call</td>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#post-api-v1-telephones">POST /api/v1/telephones</a> </td>
            <td>Used to create an 'Phone Call' journal entity.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#get-api-v1-telephones-id">GET /api/v1/telephones/{id}</a> </td>
            <td>Get details for any given 'Phone Call' journal entity in the database.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#put-api-v1-telephones-id">PUT /api/v1/telephones/{id}</a></td>
            <td>Allows you to replace a representation of the target 'Phone Call' journal entity with the request payload.</td>
        </tr>
                <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-telephones-id">DELETE /api/v1/telephones/{id}</a></td>
            <td>Allows you to permanently delete any given 'Phone Call' journal entity.</td>
        </tr>
        <tr>
            <td rowspan=4>Send CV</td>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#post-api-v1-sentcurriculumvitaes">POST /api/v1/sentcurriculumvitaes</a> </td>
            <td>Used to create an 'Send CV' journal entity.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#get-api-v1-sentcurriculumvitaes-id">GET /api/v1/sentcurriculumvitaes/{id}</a> </td>
            <td>Get details for any given 'Send CV' journal entity in the database.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#put-api-v1-sentcurriculumvitaes-id">PUT /api/v1/sentcurriculumvitaes/{id}</a></td>
            <td>Allows you to replace a representation of the target 'Send CV' journal entity with the request payload.</td>
        </tr>
                <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-sentcurriculumvitaes-id">DELETE /api/v1/sentcurriculumvitaes/{id}</a></td>
            <td>Allows you to permanently delete any given 'Send CV' journal entity.</td>
        </tr>
        <tr>
            <td rowspan=5>Task</td>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#post-api-v1-tasks">POST /api/v1/tasks</a> </td>
            <td>Used to create an 'Task' journal entity.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#get-api-v1-tasks-id">GET /api/v1/tasks/{id}</a> </td>
            <td>Get details for any given 'Task' journal entity in the database.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#get-api-v1-tasks-upcoming">GET /api/v1/tasks/upcoming</a> </td>
            <td>Get a list of upcoming 'Task' journal entities that are due in the future.</td>
        </tr>
        <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#put-api-v1-tasks-id">PUT /api/v1/tasks/{id}</a></td>
            <td>Allows you to replace a representation of the target 'Task' journal entity with the request payload.</td>
        </tr>
                <tr>
            <td><a ref="https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-tasks-id">DELETE /api/v1/tasks/{id}</a></td>
            <td>Allows you to permanently delete any given 'Task' journal entity.</td>
        </tr>
    </tbody>
</table>

## POST /api/v1/journal/list
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/journal/list' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "PageSize": 5,
  "PageIndex": 0,
  "UsePaging": true,
  "ReturnTotalCount": true,
  "ReturnTotalDatabaseItemCount": true,
    "Select": [
    "ActionSubject",
    "ActionType",
    "Notes",
    "Date"
  ],
    "Sort": [
    {
      "Selector": "Date",
      "Desc": true
    }
    ],
  "IncludeAdditionalValues": false,
  "UseLookUpViewDefinition": false,
  "IncludeDisplayViews": false,
  "IncludeAvailableColumns": false,
  "IncludeCategories": false
}'
```

> Example Response (JSON)
```shell
{
    "Items": [
        {
            "ActionSubject": "Nam faucibus ipsum nec turpis posuere fermentum. Praesent ante leo, faucibus in sodales sit amet, porta a libero.",
            "ActionType": {
                "Id": "c780dde3-b36f-4842-94a3-982cd1e59900",
                "ItemDisplayText": "Research"
            },
            "Date": "2037-11-23T00:00:00+00:00",
            "ItemType": "Telephones",
            "Notes": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent sodales nec dolor sit amet egestas. Nulla consectetur, mauris at mollis elementum, lacus purus fringilla tortor, sit amet luctus ligula nisi sed ex. Phasellus lobortis vel orci sit amet commodo. Sed non accumsan ipsum. In cursus augue quis bibendum finibus. Phasellus vel sem tempus libero laoreet ultrices. Cras eget justo odio. Sed nisi enim, scelerisque sed malesuada id, facilisis nec orci. Integer faucibus, elit ac aliquam pulvinar, dolor felis rutrum ex, vel dapibus purus nunc at felis. Donec luctus diam a ipsum pellentesque, ac suscipit erat rhoncus. Curabitur ac nibh in massa placerat interdum. Praesent non pretium justo. Praesent suscipit ante risus, condimentum ultrices felis fermentum in. Phasellus sagittis placerat purus non mollis. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Praesent semper orci ligula, sed convallis erat elementum a.",
            "ItemId": "ef403429-5cd5-44dc-ac87-2a68bfbf7a4c",
            "OffLimitsStatus": "Off"
        },
        {
            "ActionSubject": "Nam faucibus ipsum nec turpis posuere fermentum. Praesent ante leo, faucibus in sodales sit amet, porta a libero.",
            "ActionType": {
                "Id": "5f96034c-ad6a-4f7b-9ef9-441d7c15f826",
                "ItemDisplayText": "Information"
            },
            "Date": "2037-11-23T00:00:00+00:00",
            "ItemType": "Notes",
            "Notes": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent sodales nec dolor sit amet egestas. Nulla consectetur, mauris at mollis elementum, lacus purus fringilla tortor, sit amet luctus ligula nisi sed ex. Phasellus lobortis vel orci sit amet commodo. Sed non accumsan ipsum. In cursus augue quis bibendum finibus. Phasellus vel sem tempus libero laoreet ultrices. Cras eget justo odio. Sed nisi enim, scelerisque sed malesuada id, facilisis nec orci. Integer faucibus, elit ac aliquam pulvinar, dolor felis rutrum ex, vel dapibus purus nunc at felis. Donec luctus diam a ipsum pellentesque, ac suscipit erat rhoncus. Curabitur ac nibh in massa placerat interdum. Praesent non pretium justo. Praesent suscipit ante risus, condimentum ultrices felis fermentum in. Phasellus sagittis placerat purus non mollis. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Praesent semper orci ligula, sed convallis erat elementum a.",
            "ItemId": "66c8f2b9-2f6d-4564-8c58-bf7cec0b0151",
            "OffLimitsStatus": "Off"
        },
        {
            "ActionSubject": "Proin sit amet nisi enim.",
            "ActionType": {
                "Id": "5f96034c-ad6a-4f7b-9ef9-441d7c15f826",
                "ItemDisplayText": "Information"
            },
            "Date": "2037-11-22T00:00:00+00:00",
            "ItemType": "Notes",
            "Notes": "Donec diam risus, blandit at risus molestie, sodales scelerisque dui. Phasellus molestie, mi at aliquet dignissim, leo quam luctus lectus, eu euismod justo elit vel massa. Nam laoreet, felis et luctus dictum, tellus odio posuere risus, vitae consectetur sapien lorem in ipsum. Donec vulputate nunc non eros luctus consequat. Ut justo odio, dapibus pharetra risus vel, dapibus dapibus nisl. Suspendisse feugiat felis vel consequat auctor. Praesent sodales, orci at egestas euismod, ex arcu iaculis augue, vel eleifend mauris arcu in lacus. Phasellus arcu diam, eleifend quis luctus sit amet, efficitur congue erat.",
            "ItemId": "ab750b48-f990-426e-8ca1-73d287cfa2fb",
            "OffLimitsStatus": "Off"
        },
        {
            "ActionSubject": "Proin sit amet nisi enim.",
            "ActionType": {
                "Id": "c780dde3-b36f-4842-94a3-982cd1e59900",
                "ItemDisplayText": "Research"
            },
            "Date": "2037-11-22T00:00:00+00:00",
            "ItemType": "Telephones",
            "Notes": "Donec diam risus, blandit at risus molestie, sodales scelerisque dui. Phasellus molestie, mi at aliquet dignissim, leo quam luctus lectus, eu euismod justo elit vel massa. Nam laoreet, felis et luctus dictum, tellus odio posuere risus, vitae consectetur sapien lorem in ipsum. Donec vulputate nunc non eros luctus consequat. Ut justo odio, dapibus pharetra risus vel, dapibus dapibus nisl. Suspendisse feugiat felis vel consequat auctor. Praesent sodales, orci at egestas euismod, ex arcu iaculis augue, vel eleifend mauris arcu in lacus. Phasellus arcu diam, eleifend quis luctus sit amet, efficitur congue erat.",
            "ItemId": "617cf5e1-d2f6-4789-8e1a-a2f3ce7cd0c4",
            "OffLimitsStatus": "Off"
        },
        {
            "ActionSubject": "Ut vel lacinia nunc, non ullamcorper lacus.",
            "ActionType": {
                "Id": "c780dde3-b36f-4842-94a3-982cd1e59900",
                "ItemDisplayText": "Research"
            },
            "Date": "2037-11-21T00:00:00+00:00",
            "ItemType": "Telephones",
            "Notes": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi ultrices iaculis leo, eget tempus ipsum mattis at. Curabitur egestas ligula sed hendrerit fermentum. Pellentesque at interdum mauris. Cras eros ante, rutrum id enim eu, porttitor condimentum magna. In sem nunc, hendrerit vitae sagittis blandit, commodo ut arcu. Maecenas nec bibendum ante. In ut tellus arcu. Duis commodo tempor lacinia. Vivamus euismod, quam a convallis sagittis, nunc erat congue enim, viverra pretium leo tortor ut felis. Quisque sed faucibus lectus. In semper ipsum at hendrerit consectetur. Aliquam felis felis, ultricies sed feugiat id, elementum in dolor. Fusce a diam hendrerit, sodales orci et, vestibulum turpis.",
            "ItemId": "cdb27b8e-fdeb-4cf4-979a-11f9a19aa770",
            "OffLimitsStatus": "Off"
        }
    ],
    "ViewDefinition": {
        "ViewDefinitionId": "00000000-0000-0000-0000-000000000000",
        "Columns": [
            {
                "FieldName": "ActionSubject",
                "DisplayTitle": "Subject",
                "IsHidden": false,
                "Width": 100,
                "IsStaticEnum": false
            },
            {
                "FieldName": "ActionType",
                "DisplayTitle": "Action Type",
                "ColumnType": "ComboBox",
                "OptionSource": "[ActionType]",
                "IsHidden": false,
                "Width": 100,
                "IsStaticEnum": false
            },
            {
                "FieldName": "Notes",
                "DisplayTitle": "Note/Details",
                "IsHidden": false,
                "Width": 100,
                "IsStaticEnum": false
            },
            {
                "FieldName": "Date",
                "DisplayTitle": "Date & Time",
                "ColumnType": "DateTime",
                "Format": "f",
                "IsHidden": false,
                "Width": 100,
                "IsSearchEnabled": false,
                "IsStaticEnum": false
            }
        ]
    },
    "Paging": {
        "NextPageNumber": 1,
        "TotalItemCount": 44085,
        "TotalDatabaseItemCount": 44085
    }
}
```

Used to retrieve a list of global `Journal` entities in the database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/journal/list`

## POST /api/v1/appointments
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/appointments' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "Subject": "Lunch with Aaron Allan",
  "Content": "Aliquam eget risus tincidunt, tempus diam ut, blandit ipsum. Morbi iaculis fermentum urna, sit amet porttitor diam efficitur lacinia. Morbi faucibus purus at urna ultrices, sit amet mattis justo lobortis. Nunc dapibus ornare posuere. Aliquam sed orci lorem. Praesent est lorem, mollis eu turpis vel, tristique laoreet lorem. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Pellentesque id erat vulputate, convallis leo suscipit, blandit erat. Donec molestie condimentum libero et bibendum.",
  "Priority": 0,
  "StartDate": "2022-02-24T12:30:00.001Z",
  "EndDate": "2022-02-24T13:30:00.001Z",
  "AppointmentActionDate": "2022-02-23T12:21:45.001Z",
  "IsAllDayEvent": false,
  "IsReminderSet": true,
  "ReminderMinutesBeforeStart": 10,
  "Location": "Unit 1, Davidson House, Reading RG1 3EY",
  "LocationDisplayName": "Carluccio'\''s Reading",
  "LocationWebAddress": "",
  "HasAttachments": false,
  "IsMeeting": false,
  "OutlookCategory": "",
  "Attendees": "",
  "ExternalEventReference": "",
  "ShowAsBusy": true,
  "IsPrivate": false,
  "AppointmentActionType": {
    "Id": "aba667ca-4543-4b01-bedd-b0699d5646b7"
  },
  "IsPinned": false,
  "IsConfidential": false,
  "People": {
    "ItemReferences": [
      {
        "Id": "B6A53FE4-8FBD-4393-B718-AF57A92E896C"
      },
	  {
        "Id": "D7A4D92C-04D0-472B-8BAC-514E5CDFACF3"
      }
    ]
  }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "778571c8-40a7-4c3c-adeb-2ebec32bce5b",
    "EntityDetails": {
        "DateCreated": "2022-02-24T16:03:59.9489482+00:00",
        "DateModified": "2022-02-24T16:03:59.9489482+00:00",
        "CreatedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Lunch with Aaron Allan",
    "Content": "Aliquam eget risus tincidunt, tempus diam ut, blandit ipsum. Morbi iaculis fermentum urna, sit amet porttitor diam efficitur lacinia. Morbi faucibus purus at urna ultrices, sit amet mattis justo lobortis. Nunc dapibus ornare posuere. Aliquam sed orci lorem. Praesent est lorem, mollis eu turpis vel, tristique laoreet lorem. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Pellentesque id erat vulputate, convallis leo suscipit, blandit erat. Donec molestie condimentum libero et bibendum.",
    "Priority": 0,
    "StartDate": "2022-02-24T12:30:00.001+00:00",
    "EndDate": "2022-02-24T13:30:00.001+00:00",
    "AppointmentActionDate": "2022-02-23T12:21:45.001+00:00",
    "IsAllDayEvent": false,
    "IsReminderSet": true,
    "ReminderMinutesBeforeStart": 10,
    "Location": "Unit 1, Davidson House, Reading RG1 3EY",
    "LocationDisplayName": "Carluccio's Reading",
    "LocationWebAddress": "",
    "HasAttachments": false,
    "IsMeeting": false,
    "OutlookCategory": "",
    "Attendees": "",
    "ExternalEventReference": "",
    "ShowAsBusy": true,
    "IsPrivate": false,
    "AppointmentActionType": {
        "Id": "aba667ca-4543-4b01-bedd-b0699d5646b7",
        "DisplayTitle": "Appointment Action Type",
        "ItemDisplayText": "Initial Meeting",
        "ItemType": "LookupListEntries"
    },
    "Programmes": [],
    "Notes": [],
    "Telephones": [],
    "Emails": [],
    "SentDocuments": [],
    "Interviews": [],
    "MeetingNotes": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "d7a4d92c-04d0-472b-8bac-514e5cdfacf3",
            "ItemDisplayText": "Mr Aaron Allan",
            "ItemType": "People"
        },
        {
            "Id": "b6a53fe4-8fbd-4393-b718-af57a92e896c",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": []
}
```

Used to create an `Appointment` journal entity in the database.

<aside class="notice">
Please take note of the distinction between an appointment and a meeting in the context of this application. Appointments are intended for individual scheduling, while meetings are used to invite multiple participants.

Furthermore, it is essential to understand that this functionality is specifically designed for adding historical data. It does not involve adding the appointment or meeting to end-users' calendars, nor does it send invitations to recipients. Rather, its primary purpose is to log the activity in the database for reference and record-keeping.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/appoinments`

## POST /api/v1/appointments/list
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/appointments/list' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "PageSize": 5,
  "PageIndex": 0,
  "UsePaging": true,
  "ReturnTotalCount": true,
  "ReturnTotalDatabaseItemCount": true,
    "Select": [
    "ActionSubject",
    "ActionType",
    "Content",
    "Date"
  ],
    "Sort": [
    {
      "Selector": "Date",
      "Desc": true
    }
    ],
  "IncludeAdditionalValues": false,
  "UseLookUpViewDefinition": false,
  "IncludeDisplayViews": false,
  "IncludeAvailableColumns": false,
  "IncludeCategories": false
}'
```

> Example Response (JSON)
```shell
{
    "Items": [
        {
            "Content": "Ut sagittis, nunc molestie mattis maximus, nisi turpis aliquet ipsum, at posuere nibh dui in leo.",
            "Date": "2023-01-27T15:45:01.001+00:00",
            "ItemType": "Appointments",
            "ItemId": "75c2a51b-7fee-4763-b7e4-03b8ec0dd41d",
            "OffLimitsStatus": "Off"
        },
        {
            "Content": "Cras euismod bibendum nisl, a vestibulum ligula feugiat vel. Curabitur sagittis laoreet lectus, quis luctus diam lobortis finibus.",
            "Date": "2023-01-27T15:20:01.001+00:00",
            "ItemType": "Appointments",
            "ItemId": "46186ecb-efd9-4067-8b5d-cbf7216ef83f",
            "OffLimitsStatus": "Off"
        },
        {
            "Content": "Quisque rutrum felis ac ligula sodales pellentesque.",
            "Date": "2023-01-26T14:30:01.001+00:00",
            "ItemType": "Appointments",
            "ItemId": "bd9a64cb-2c9c-4452-b47b-53da6fc4387a",
            "OffLimitsStatus": "Off"
        },
        {
            "Content": "Suspendisse eu purus at nibh blandit feugiat. Integer tristique dui aliquet pellentesque viverra. Curabitur non tortor quis nunc tempus finibus.",
            "Date": "2023-01-26T14:10:01.001+00:00",
            "ItemType": "Appointments",
            "ItemId": "9220ee15-95d5-451f-ae13-0fe49a78530b",
            "OffLimitsStatus": "Off"
        },
        {
            "Content": "Sed sollicitudin ultrices rutrum. Suspendisse et magna commodo, cursus mauris consequat, aliquam purus. In a dictum lorem. Ut nec faucibus tortor.",
            "Date": "2023-01-25T13:15:01.001+00:00",
            "ItemType": "Appointments",
            "ItemId": "7b09a769-2ce3-49b6-8ee8-7a83111e7c43",
            "OffLimitsStatus": "Off"
        }
    ],
    "ViewDefinition": {
        "ViewDefinitionId": "00000000-0000-0000-0000-000000000000",
        "Columns": [
            {
                "FieldName": "ActionSubject",
                "IsStaticEnum": false
            },
            {
                "FieldName": "ActionType",
                "IsStaticEnum": false
            },
            {
                "FieldName": "Content",
                "DisplayTitle": "Note/Details",
                "IsHidden": false,
                "Width": 100,
                "IsStaticEnum": false
            },
            {
                "FieldName": "Date",
                "DisplayTitle": "Date & Time",
                "ColumnType": "DateTime",
                "Format": "f",
                "IsHidden": false,
                "Width": 100,
                "IsSearchEnabled": false,
                "IsStaticEnum": false
            }
        ]
    },
    "Paging": {
        "NextPageNumber": 1,
        "TotalItemCount": 7997,
        "TotalDatabaseItemCount": 7997
    }
}
```
Get a list of `Appointment` journal entities in the database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/appointments/list`

## GET /api/v1/appointments/upcoming
> Example (cURL)
```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/appointments/upcoming' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
[
    {
        "Id": "b7eba2cf-8633-44ec-88c0-a387790c27d6",
        "DisplayTitle": "Lunch with Thomas Hyde (Wisoky Ltd)",
        "DueDateTime": "2022-02-25T15:10:01.001+00:00"
    },
    {
        "Id": "1a5ef511-f7a4-425c-b02a-a2bbe4bbe25a",
        "DisplayTitle": "Coffee with Louis Matthews (Sveinmars hf.)",
        "DueDateTime": "2022-02-25T15:40:01.001+00:00"
    },
    {
        "Id": "0671ec9a-2882-4315-a8f5-d2a884d0685f",
        "DisplayTitle": "Lunch with Rhys Randall (Wisoky Ltd)",
        "DueDateTime": "2022-02-26T12:20:01.001+00:00"
    },
    {
        "Id": "3eb47464-6640-4ccd-8ede-b518940f4be7",
        "DisplayTitle": "Lunch with Christopher Johnson (Sveinmars hf.)",
        "DueDateTime": "2022-02-26T12:50:01.001+00:00"
    },
    {
        "Id": "3d371331-3801-4733-b652-3d94d9ea29cd",
        "DisplayTitle": "Lunch with Paul Freeman (Sveinmars hf.)",
        "DueDateTime": "2022-02-27T13:15:01.001+00:00"
    }
]
```

Get the next five upcoming `Appointment` journal entities scheduled to take place in the future.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/appointments/upcoming`

## GET /api/v1/appointments/{id}
> Example (cURL)
```shell
curl --location --request GET 'https://atlasexecutivesearch.miginvenias.com/api/v1/appointments/{id}' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
{
    "Id": "b7eba2cf-8633-44ec-88c0-a387790c27d6",
    "EntityDetails": {
        "DateCreated": "2022-02-25T14:13:57.6667517+00:00",
        "DateModified": "2022-02-25T14:13:57.6667517+00:00",
        "CreatedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Lunch with Thomas Hyde (Wisoky Ltd)",
    "Content": "Morbi urna massa, aliquam tincidunt ante sed, tempus consectetur mauris. Nulla facilisi.",
    "Priority": 0,
    "StartDate": "2022-02-25T15:10:01.001+00:00",
    "EndDate": "2022-02-25T16:10:01.001+00:00",
    "AppointmentActionDate": "2022-02-25T15:10:01.001+00:00",
    "IsAllDayEvent": false,
    "IsReminderSet": true,
    "ReminderMinutesBeforeStart": 10,
    "Location": "Bondi Green - London",
    "LocationDisplayName": "404 Canal Side Walk, 2 Bishop's Bridge Rd, London W2 1DG",
    "LocationWebAddress": "",
    "HasAttachments": false,
    "IsMeeting": false,
    "OutlookCategory": "",
    "Attendees": "",
    "ExternalEventReference": "",
    "ShowAsBusy": true,
    "IsPrivate": false,
    "AppointmentActionType": {
        "Id": "aba667ca-4543-4b01-bedd-b0699d5646b7",
        "DisplayTitle": "Appointment Action Type",
        "ItemDisplayText": "Initial Meeting",
        "ItemType": "LookupListEntries"
    },
    "Programmes": [],
    "Notes": [],
    "Telephones": [],
    "Emails": [],
    "SentDocuments": [],
    "Interviews": [],
    "MeetingNotes": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "444ad5f2-1f21-4495-bb61-22213e6f7c4d",
            "ItemDisplayText": "Mr Thomas Hyde",
            "ItemType": "People"
        },
        {
            "Id": "b6a53fe4-8fbd-4393-b718-af57a92e896c",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": []
}
```

Get details for any given `Appointment` journal entity in the database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/appointments/{id}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Appointment` journal entity.

## PUT /api/v1/appointments/{id}
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.miginvenias.com/api/v1/appointments/{id}' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Subject": "Lunch with Thomas Hyde (Wisoky Ltd)",
    "Content": "Morbi urna massa, aliquam tincidunt ante sed, tempus consectetur mauris. Nulla facilisi.",
    "Priority": 0,
    "StartDate": "2022-02-26T15:10:01.001+00:00",
    "EndDate": "2022-02-26T16:10:01.001+00:00",
    "AppointmentActionDate": "2022-02-25T15:10:01.001+00:00",
    "IsAllDayEvent": false,
    "IsReminderSet": true,
    "ReminderMinutesBeforeStart": 10,
    "Location": "Bondi Green - London",
    "LocationDisplayName": "404 Canal Side Walk, 2 Bishop'\''s Bridge Rd, London W2 1DG",
    "LocationWebAddress": "",
    "HasAttachments": false,
    "IsMeeting": false,
    "OutlookCategory": "",
    "Attendees": "",
    "ExternalEventReference": "",
    "ShowAsBusy": true,
    "IsPrivate": false,
    "AppointmentActionType": {
        "Id": "aba667ca-4543-4b01-bedd-b0699d5646b7",
        "DisplayTitle": "Appointment Action Type",
        "ItemDisplayText": "Initial Meeting",
        "ItemType": "LookupListEntries"
    },
    "IsPinned": false,
    "IsConfidential": false,
    "People": {
        "ItemReferences": [
            {
                "Id": "444ad5f2-1f21-4495-bb61-22213e6f7c4d",
                "ItemDisplayText": "Mr Thomas Hyde",
                "ItemType": "People"
            },
            {
                "Id": "b6a53fe4-8fbd-4393-b718-af57a92e896c",
                "ItemDisplayText": "Glen Chamberlain",
                "ItemType": "People"
            }
        ]
    }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "b7eba2cf-8633-44ec-88c0-a387790c27d6",
    "EntityDetails": {
        "DateCreated": "2022-02-25T14:13:57.6667517+00:00",
        "DateModified": "2022-02-25T15:25:14.8569075+00:00",
        "CreatedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Lunch with Thomas Hyde (Wisoky Ltd)",
    "Content": "Morbi urna massa, aliquam tincidunt ante sed, tempus consectetur mauris. Nulla facilisi.",
    "Priority": 0,
    "StartDate": "2022-02-26T15:10:01.001+00:00",
    "EndDate": "2022-02-26T16:10:01.001+00:00",
    "AppointmentActionDate": "2022-02-25T15:10:01.001+00:00",
    "IsAllDayEvent": false,
    "IsReminderSet": true,
    "ReminderMinutesBeforeStart": 10,
    "Location": "Bondi Green - London",
    "LocationDisplayName": "404 Canal Side Walk, 2 Bishop's Bridge Rd, London W2 1DG",
    "LocationWebAddress": "",
    "HasAttachments": false,
    "IsMeeting": false,
    "OutlookCategory": "",
    "Attendees": "",
    "ExternalEventReference": "",
    "ShowAsBusy": true,
    "IsPrivate": false,
    "AppointmentActionType": {
        "Id": "aba667ca-4543-4b01-bedd-b0699d5646b7",
        "DisplayTitle": "Appointment Action Type",
        "ItemDisplayText": "Initial Meeting",
        "ItemType": "LookupListEntries"
    },
    "Programmes": [],
    "Notes": [],
    "Telephones": [],
    "Emails": [],
    "SentDocuments": [],
    "Interviews": [],
    "MeetingNotes": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "444ad5f2-1f21-4495-bb61-22213e6f7c4d",
            "ItemDisplayText": "Mr Thomas Hyde",
            "ItemType": "People"
        },
        {
            "Id": "b6a53fe4-8fbd-4393-b718-af57a92e896c",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": []
}
```

Allows you to replace a representation of the target `Appointment` journal entity with the request payload.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/appointments/{id}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Appointment` journal entity.

## DELETE /api/v1/appointments/{id}
> Example (cURL)
```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/appointments/{id}' \
--header 'Authorization: Bearer {token}'
```

> Please note, successful requests will return a 200 OK response code.

Allows you to permanently delete any given `Appointment` journal entity.


### HTTP Request
`https://{subdomain}.invenias.com/api/v1/appointments/{id}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Appointment` journal entity.

## POST /api/v1/sentdocuments
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/sentdocuments' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Subject": "Terms of Business Template",
    "Body": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec a convallis velit, eu posuere purus. Praesent sagittis convallis mauris quis scelerisque. Nullam sit amet efficitur eros, in tempor ante.",
    "SentDocumentActionDate": "2022-02-25T14:31:50.888Z",
    "SentDocumentActionType": {
        "Id": "1462f961-ebcd-4b8d-87b6-c1d7bd8a8064"
    },
    "Documents": {
        "ItemReferences": [
            {
                "Id": "200e3281-9eb7-47bf-b151-2568f54dc810"
            }
        ]
    },
    "AddressedToPeople": {
        "ItemReferences": [
            {
                "Id": "5ED2FB3B-5751-45D7-96C6-F36D73CFDABA"
            }
        ]
    },
    "IsPinned": true,
    "IsConfidential": true,
    "People": {
        "ItemReferences": [
            {
                "Id": "5ED2FB3B-5751-45D7-96C6-F36D73CFDABA"
            }
        ]
    },
    "Companies": {
        "ItemReferences": [
            {
                "Id": "D9C789A4-AD79-44F9-978F-013C670A1207"
            }
        ]
    }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "a1df0675-c6eb-4dc0-a0be-beff3c07d7b8",
    "EntityDetails": {
        "DateCreated": "2022-02-25T16:03:41.8802247+00:00",
        "DateModified": "2022-02-25T16:03:41.8802247+00:00",
        "CreatedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Terms of Business Template",
    "Body": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec a convallis velit, eu posuere purus. Praesent sagittis convallis mauris quis scelerisque. Nullam sit amet efficitur eros, in tempor ante.",
    "SentDocumentActionDate": "2022-02-25T14:31:50.888+00:00",
    "SentDocumentActionType": {
        "Id": "1462f961-ebcd-4b8d-87b6-c1d7bd8a8064",
        "DisplayTitle": "Sent Document Action Type",
        "ItemDisplayText": "Terms of Business",
        "ItemType": "LookupListEntries"
    },
    "Programmes": [],
    "Appointments": [],
    "Tasks": [],
    "CandidateInterviews": [],
    "ClientInterviews": [],
    "SentCurriculumVitaes": [],
    "Documents": [
        {
            "Id": "200e3281-9eb7-47bf-b151-2568f54dc810",
            "ItemDisplayText": "Atlas-Executive-Search-Terms-of-Business.docx",
            "ItemType": "Documents"
        }
    ],
    "AddressedToPeople": [
        {
            "Id": "5ed2fb3b-5751-45d7-96c6-f36d73cfdaba",
            "ItemDisplayText": "Mr Aaron Bryant",
            "ItemType": "People"
        }
    ],
    "AddressedToCompanies": [],
    "IsPinned": true,
    "IsConfidential": true,
    "People": [
        {
            "Id": "5ed2fb3b-5751-45d7-96c6-f36d73cfdaba",
            "ItemDisplayText": "Mr Aaron Bryant",
            "ItemType": "People"
        }
    ],
    "Companies": [
        {
            "Id": "d9c789a4-ad79-44f9-978f-013c670a1207",
            "ItemDisplayText": "Feil-Crona",
            "ItemType": "Companies"
        }
    ],
    "Assignments": []
}
```
Used to create an `Sent Document` journal entity in the database.

<aside class="notice">
It is essential to understand that this functionality is specifically designed for adding historical data. It does not send out a document via the end-users email. Rather, its primary purpose is to log the activity in the database for reference and record-keeping.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/sentdocuments`

## GET /api/v1/sentdocuments/{id}
> Example (cURL)
```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/sentdocuments/{id}' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
{
    "Id": "a1df0675-c6eb-4dc0-a0be-beff3c07d7b8",
    "EntityDetails": {
        "DateCreated": "2022-02-25T16:03:41.8802247+00:00",
        "DateModified": "2022-02-25T16:03:41.8802247+00:00",
        "CreatedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Terms of Business Template",
    "Body": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec a convallis velit, eu posuere purus. Praesent sagittis convallis mauris quis scelerisque. Nullam sit amet efficitur eros, in tempor ante.",
    "SentDocumentActionDate": "2022-02-25T14:31:50.888+00:00",
    "SentDocumentActionType": {
        "Id": "1462f961-ebcd-4b8d-87b6-c1d7bd8a8064",
        "DisplayTitle": "Sent Document Action Type",
        "ItemDisplayText": "Terms of Business",
        "ItemType": "LookupListEntries"
    },
    "Programmes": [],
    "Appointments": [],
    "Tasks": [],
    "CandidateInterviews": [],
    "ClientInterviews": [],
    "SentCurriculumVitaes": [],
    "Documents": [
        {
            "Id": "200e3281-9eb7-47bf-b151-2568f54dc810",
            "ItemDisplayText": "Atlas-Executive-Search-Terms-of-Business.docx",
            "ItemType": "Documents"
        }
    ],
    "AddressedToPeople": [
        {
            "Id": "5ed2fb3b-5751-45d7-96c6-f36d73cfdaba",
            "ItemDisplayText": "Mr Aaron Bryant",
            "ItemType": "People"
        }
    ],
    "AddressedToCompanies": [],
    "IsPinned": true,
    "IsConfidential": true,
    "People": [
        {
            "Id": "5ed2fb3b-5751-45d7-96c6-f36d73cfdaba",
            "ItemDisplayText": "Mr Aaron Bryant",
            "ItemType": "People"
        }
    ],
    "Companies": [
        {
            "Id": "d9c789a4-ad79-44f9-978f-013c670a1207",
            "ItemDisplayText": "Feil-Crona",
            "ItemType": "Companies"
        }
    ],
    "Assignments": []
}
```

Get details for any given `Sent Document` journal entity in the database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/sentdocuments/{id}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Sent Document` journal entity.

## PUT /api/v1/sentdocuments/{id}
> Example (cURL)
```shell
curl --location --request PUT 'https://atlasexecutivesearch.miginvenias.com/api/v1/sentdocuments/a1df0675-c6eb-4dc0-a0be-beff3c07d7b8' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Subject": "Terms of Business Template",
    "Body": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec a convallis velit, eu posuere purus. Praesent sagittis convallis mauris quis scelerisque. Nullam sit amet efficitur eros, in tempor ante.",
    "SentDocumentActionDate": "2022-02-25T14:31:50.888Z",
    "SentDocumentActionType": {
        "Id": "1462f961-ebcd-4b8d-87b6-c1d7bd8a8064"
    },
    "Documents": {
        "ItemReferences": [
            {
                "Id": "200e3281-9eb7-47bf-b151-2568f54dc810"
            }
        ]
    },
    "AddressedToPeople": {
        "ItemReferences": [
            {
                "Id": "5ED2FB3B-5751-45D7-96C6-F36D73CFDABA"
            }
        ]
    },
    "IsPinned": true,
    "IsConfidential": true,
    "People": {
        "ItemReferences": [
            {
                "Id": "5ED2FB3B-5751-45D7-96C6-F36D73CFDABA"
            }
        ]
    },
    "Companies": {
        "ItemReferences": [
            {
                "Id": "D9C789A4-AD79-44F9-978F-013C670A1207"
            }
        ]
    },
    "Owner": {
        "Id": "f83a3a69-aa6a-4bc1-943d-039eac61a808"
    }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "a1df0675-c6eb-4dc0-a0be-beff3c07d7b8",
    "EntityDetails": {
        "DateCreated": "2022-02-25T16:03:41.8802247+00:00",
        "DateModified": "2022-02-28T10:27:58.7605724+00:00",
        "CreatedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "f83a3a69-aa6a-4bc1-943d-039eac61a808",
            "ItemDisplayText": "Peter Clark",
            "ItemType": "Users"
        }
    },
    "Subject": "Terms of Business Template",
    "Body": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec a convallis velit, eu posuere purus. Praesent sagittis convallis mauris quis scelerisque. Nullam sit amet efficitur eros, in tempor ante.",
    "SentDocumentActionDate": "2022-02-25T14:31:50.888+00:00",
    "SentDocumentActionType": {
        "Id": "1462f961-ebcd-4b8d-87b6-c1d7bd8a8064",
        "DisplayTitle": "Sent Document Action Type",
        "ItemDisplayText": "Terms of Business",
        "ItemType": "LookupListEntries"
    },
    "Programmes": [],
    "Appointments": [],
    "Tasks": [],
    "CandidateInterviews": [],
    "ClientInterviews": [],
    "SentCurriculumVitaes": [],
    "Documents": [
        {
            "Id": "200e3281-9eb7-47bf-b151-2568f54dc810",
            "ItemDisplayText": "Atlas-Executive-Search-Terms-of-Business.docx",
            "ItemType": "Documents"
        }
    ],
    "AddressedToPeople": [
        {
            "Id": "5ed2fb3b-5751-45d7-96c6-f36d73cfdaba",
            "ItemDisplayText": "Mr Aaron Bryant",
            "ItemType": "People"
        }
    ],
    "AddressedToCompanies": [],
    "IsPinned": true,
    "IsConfidential": true,
    "People": [
        {
            "Id": "5ed2fb3b-5751-45d7-96c6-f36d73cfdaba",
            "ItemDisplayText": "Mr Aaron Bryant",
            "ItemType": "People"
        }
    ],
    "Companies": [
        {
            "Id": "d9c789a4-ad79-44f9-978f-013c670a1207",
            "ItemDisplayText": "Feil-Crona",
            "ItemType": "Companies"
        }
    ],
    "Assignments": []
}
```

Allows you to replace a representation of the target `Sent Document` journal entity with the request payload.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/sentdocuments/{id}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Sent Document` journal entity.

## DELETE /api/v1/sentdocuments/{id}
> Example (cURL)
```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/sentdocuments/{id}' \
--header 'Authorization: Bearer {token}'
```

> Please note, successful requests will return a 200 OK response code.

Allows you to permanently delete any given `Sent Document` journal entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/sentdocuments/{id}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Sent Document` journal entity.

## POST /api/v1/emails
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/emails' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Recipients": [
        {
            "EmailAddress": "Aaron.Allan@emailaddress1.com",
            "PersonId": "5ED2FB3B-5751-45D7-96C6-F36D73CFDABA"
        }
    ],
    "Subject": "Subject: Introduction from Jane Doe",
    "ToAddress": "Aaron.Allan@emailaddress1.com",
    "Body": "Dear Aaron,\r\n\r\nOne of my dear friends and our mutual acquaintance John Doe very enthusiastically recommended your name for a Chief Security Officer role that I’m looking to fill for a client.\r\n\r\nThey have been ranked as one of the fastest-growing startups in the Consumer Goods industry and this role will play a key part in their client acquisition and retention activities. They are looking for someone who is obsessed with security and can contribute to the continued success of the business.\r\n\r\nDo let me know if you are interested in discussing the role and company in more detail.\r\n\r\nI am flexible with my availability. Thank you for your time.\r\n\r\nGlen Chamberlain",
    "SenderName": "Glen Chamberlain",
    "IsSent": false,
    "EmailActionType": {
        "Id": "655aa355-1a8c-4ff3-8a80-7199a203fb9b"
    },
    "IsPinned": false,
    "IsConfidential": false,
    "People": {
        "ItemReferences": [
            {
                "Id": "B6A53FE4-8FBD-4393-B718-AF57A92E896C"
            },
            {
                "Id": "D7A4D92C-04D0-472B-8BAC-514E5CDFACF3"
            }
        ]
    }
}'
```

> Example Response (JSON)
```shell
{
    "Recipients": [
        {
            "EmailAddress": "Aaron.Allan@emailaddress1.com",
            "PersonId": "5ED2FB3B-5751-45D7-96C6-F36D73CFDABA"
        }
    ],
    "Subject": "Subject: Introduction from Jane Doe",
    "ToAddress": "Aaron.Allan@emailaddress1.com",
    "Body": "Dear Aaron,\r\n\r\nOne of my dear friends and our mutual acquaintance John Doe very enthusiastically recommended your name for a Chief Security Officer role that I’m looking to fill for a client.\r\n\r\nThey have been ranked as one of the fastest-growing startups in the Consumer Goods industry and this role will play a key part in their client acquisition and retention activities. They are looking for someone who is obsessed with security and can contribute to the continued success of the business.\r\n\r\nDo let me know if you are interested in discussing the role and company in more detail.\r\n\r\nI am flexible with my availability. Thank you for your time.\r\n\r\nGlen Chamberlain",
    "SenderName": "Glen Chamberlain",
    "IsSent": true,
    "EmailActionType": {
        "Id": "655aa355-1a8c-4ff3-8a80-7199a203fb9b"
    },
    "IsPinned": false,
    "IsConfidential": false,
    "People": {
        "ItemReferences": [
            {
                "Id": "B6A53FE4-8FBD-4393-B718-AF57A92E896C"
            },
            {
                "Id": "D7A4D92C-04D0-472B-8BAC-514E5CDFACF3"
            }
        ]
    }
}
```
<aside class="notice">
Please note, this endpoint will not allow you to save the actual email. It is used to create a record of the event in the journal only. You can add the body of the email in plain text, which will be visible when viewing the journal entity via Invenias applications.
</aside>

<aside class="warning">
Please note, for the and email to appear correctly in journals you must include the 'IsSent' attribute in the payload with the value "true".
</aside>

Used to create an `Email` journal entity in the database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/emails`

## GET /api/v1/emails/{id}
> Example (cURL)
```shell
curl --location --request GET 'https://atlasexecutivesearch.miginvenias.com/api/v1/emails/{id}' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
{
    "Id": "7ec655e7-0e92-4ad4-a1da-e299f75d5601",
    "Subject": "Subject: Introduction from Jane Doe",
    "ToAddress": "Aaron.Allan@emailaddress1.com",
    "Body": "Dear Aaron,\r\n\r\nOne of my dear friends and our mutual acquaintance John Doe very enthusiastically recommended your name for a Chief Security Officer role that I’m looking to fill for a client.\r\n\r\nThey have been ranked as one of the fastest-growing startups in the Consumer Goods industry and this role will play a key part in their client acquisition and retention activities. They are looking for someone who is obsessed with security and can contribute to the continued success of the business.\r\n\r\nDo let me know if you are interested in discussing the role and company in more detail.\r\n\r\nI am flexible with my availability. Thank you for your time.\r\n\r\nGlen Chamberlain",
    "SenderName": "Glen Chamberlain",
    "IsSent": false,
    "EmailActionType": {
        "Id": "655aa355-1a8c-4ff3-8a80-7199a203fb9b",
        "DisplayTitle": "Email Action Type",
        "ItemDisplayText": "Candidate Contact",
        "ItemType": "LookupListEntries"
    },
    "SaveConversationThread": false,
    "Programmes": [],
    "Appointments": [],
    "Tasks": [],
    "CandidateInterviews": [],
    "ClientInterviews": [],
    "ClientSentCurriculumVitaes": [],
    "Documents": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "d7a4d92c-04d0-472b-8bac-514e5cdfacf3",
            "ItemDisplayText": "Mr Aaron Allan",
            "ItemType": "People"
        },
        {
            "Id": "b6a53fe4-8fbd-4393-b718-af57a92e896c",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": [],
    "Owner": {
        "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
        "ItemDisplayText": "Glen Chamberlain",
        "ItemType": "Users"
    }
}
```

Get details for any given `Email` journal entity in the database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/emails/{id}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Email` journal entity.


## PUT /api/v1/emails/{id}
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/emails/{id}' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Owner": {
        "Id": "f83a3a69-aa6a-4bc1-943d-039eac61a808"
    },
    "Recipients": [
        {
            "EmailAddress": "Aaron.Allan@emailaddress1.com",
            "PersonId": "5ED2FB3B-5751-45D7-96C6-F36D73CFDABA"
        }
    ],
    "Subject": "Subject: Introduction from Jane Doe",
    "ToAddress": "Aaron.Allan@emailaddress1.com",
    "Body": "Dear Aaron,\r\n\r\nOne of my dear friends and our mutual acquaintance John Doe very enthusiastically recommended your name for a Chief Security Officer role that I’m looking to fill for a client.\r\n\r\nThey have been ranked as one of the fastest-growing startups in the Consumer Goods industry and this role will play a key part in their client acquisition and retention activities. They are looking for someone who is obsessed with security and can contribute to the continued success of the business.\r\n\r\nDo let me know if you are interested in discussing the role and company in more detail.\r\n\r\nI am flexible with my availability. Thank you for your time.\r\n\r\nGlen Chamberlain",
    "SenderName": "Glen Chamberlain",
    "IsSent": false,
    "EmailActionType": {
        "Id": "655aa355-1a8c-4ff3-8a80-7199a203fb9b"
    },
    "IsPinned": false,
    "IsConfidential": false,
    "People": {
        "ItemReferences": [
            {
                "Id": "B6A53FE4-8FBD-4393-B718-AF57A92E896C"
            },
            {
                "Id": "D7A4D92C-04D0-472B-8BAC-514E5CDFACF3"
            }
        ]
    }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "7ec655e7-0e92-4ad4-a1da-e299f75d5601",
    "Subject": "Subject: Introduction from Jane Doe",
    "ToAddress": "Aaron.Allan@emailaddress1.com",
    "Body": "Dear Aaron,\r\n\r\nOne of my dear friends and our mutual acquaintance John Doe very enthusiastically recommended your name for a Chief Security Officer role that I’m looking to fill for a client.\r\n\r\nThey have been ranked as one of the fastest-growing startups in the Consumer Goods industry and this role will play a key part in their client acquisition and retention activities. They are looking for someone who is obsessed with security and can contribute to the continued success of the business.\r\n\r\nDo let me know if you are interested in discussing the role and company in more detail.\r\n\r\nI am flexible with my availability. Thank you for your time.\r\n\r\nGlen Chamberlain",
    "SenderName": "Glen Chamberlain",
    "IsSent": false,
    "EmailActionType": {
        "Id": "655aa355-1a8c-4ff3-8a80-7199a203fb9b",
        "DisplayTitle": "Email Action Type",
        "ItemDisplayText": "Candidate Contact",
        "ItemType": "LookupListEntries"
    },
    "SaveConversationThread": false,
    "Programmes": [],
    "Appointments": [],
    "Tasks": [],
    "CandidateInterviews": [],
    "ClientInterviews": [],
    "ClientSentCurriculumVitaes": [],
    "Documents": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "d7a4d92c-04d0-472b-8bac-514e5cdfacf3",
            "ItemDisplayText": "Mr Aaron Allan",
            "ItemType": "People"
        },
        {
            "Id": "b6a53fe4-8fbd-4393-b718-af57a92e896c",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": [],
    "Owner": {
        "Id": "f83a3a69-aa6a-4bc1-943d-039eac61a808",
        "ItemDisplayText": "Peter Clark",
        "ItemType": "Users"
    }
}
```

Allows you to replace a representation of the target `Email` journal entity with the request payload.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/emails/{id}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Email` journal entity.

## DELETE /api/v1/emails/{id}
> Example (cURL)
```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/emails/{id}' \
--header 'Authorization: Bearer {token}'
```

> Please note, successful requests will return a 200 OK response code.

Allows you to permanently delete any given `Email` journal entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/emails/{id}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Email` journal entity.

## POST /api/v1/feedback
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/feedback' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "FeedbackActionType": {
    "Id": "0CA6F019-D8EC-46C1-B2FE-19A1E2A2D709"
  },
  "Content": "Morbi laoreet velit id vestibulum vehicula. Proin nec felis eget velit aliquam malesuada nec et sapien. Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
  "IsPublished": false,
  "IsInternal": false,
  "FeedbackActionDate": "2016-01-01T00:01:01.001Z",
  "TargetFeedback": {
    "ItemReferences": [
      {
        "Id": "4F90A43C-4FCB-495D-A3F9-001130523CE7"
      }
    ]
  },
  "IsPinned": false,
  "IsConfidential": false,
  "People": {
    "ItemReferences": [
      {
        "Id": "B6A53FE4-8FBD-4393-B718-AF57A92E896C"
      }
    ]
  }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "21c3c2d5-1541-4a14-96d9-722dceeb2d94",
    "FeedbackComments": [],
    "EntityDetails": {
        "DateCreated": "2022-03-07T15:49:13.0247178+00:00",
        "DateModified": "2022-03-07T15:49:13.0247178+00:00",
        "CreatedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "FeedbackActionType": {
        "Id": "0ca6f019-d8ec-46c1-b2fe-19a1e2a2d709",
        "DisplayTitle": "FeedBack Action Type",
        "ItemDisplayText": "General Feedback",
        "ItemType": "LookupListEntries"
    },
    "Content": "Morbi laoreet velit id vestibulum vehicula. Proin nec felis eget velit aliquam malesuada nec et sapien. Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
    "IsPublished": false,
    "IsInternal": true,
    "FeedbackActionDate": "2016-01-01T00:01:01.001+00:00",
    "Programmes": [],
    "TargetFeedback": [
        {
            "Id": "4f90a43c-4fcb-495d-a3f9-001130523ce7",
            "ItemDisplayText": "Mr Ezra Forster",
            "ItemType": "People"
        }
    ],
    "Appointments": [],
    "Emails": [],
    "Interviews": [],
    "Notes": [],
    "SentCurriculumVitaes": [],
    "SentDocuments": [],
    "Tasks": [],
    "Telephones": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "b6a53fe4-8fbd-4393-b718-af57a92e896c",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": []
}
```

Used to create an `Feedback` journal entity for any given `Person` entity in the database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/feedback`


## POST /api/v1/feedback/{id}/comments
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/feedback' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw 'curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/feedback/8e3ade0d-e6cf-4ee8-9d9e-5905c9c3d20d/comments' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "Content": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Maecenas nec bibendum ex, vel venenatis tortor. Donec elementum urna ac faucibus suscipit. Aliquam pulvinar augue venenatis euismod malesuada.",
  "IsPublished": true,
  "CommentBy": {
    "Id": "fbc2a562-90fe-4765-bf8a-80859fdeb916"
  }
}
```

> Example Response (JSON)
```shell
{
    "Id": "a7028388-986e-47df-a2d8-2e3bd6bcf723",
    "EntityDetails": {
        "DateCreated": "2022-07-19T13:06:01.6371371+00:00",
        "DateModified": "2022-07-19T13:06:01.6371371+00:00",
        "CreatedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Content": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Maecenas nec bibendum ex, vel venenatis tortor. Donec elementum urna ac faucibus suscipit. Aliquam pulvinar augue venenatis euismod malesuada.",
    "IsPublished": true,
    "CommentBy": {
        "Id": "fbc2a562-90fe-4765-bf8a-80859fdeb916",
        "DisplayTitle": "Comment By",
        "ItemDisplayText": "Aaron Davidson",
        "ItemType": "People"
    }
}
```

Used to add a comment from `Person` journal entity to an existing `Feedback` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/feedback/{id}/comments`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Feedback` journal entity you wish to add a commment to.
IsPublished (optional) | false | Specify whether the feedback comment should be visible on the Invenias Client web application to Client Users.

## GET /api/v1/feedback/{id}
> Example (cURL)
```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/feedback/8e3ade0d-e6cf-4ee8-9d9e-5905c9c3d20d' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
{
    "Id": "8e3ade0d-e6cf-4ee8-9d9e-5905c9c3d20d",
    "FeedbackComments": [
        {
            "Id": "a7028388-986e-47df-a2d8-2e3bd6bcf723",
            "EntityDetails": {
                "DateCreated": "2022-07-19T13:06:01.6371371+00:00",
                "DateModified": "2022-07-19T13:06:01.6371371+00:00",
                "CreatedBy": {
                    "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
                    "ItemDisplayText": "Glen Chamberlain",
                    "ItemType": "Users"
                },
                "ModifiedBy": {
                    "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
                    "ItemDisplayText": "Glen Chamberlain",
                    "ItemType": "Users"
                },
                "Owner": {
                    "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
                    "ItemDisplayText": "Glen Chamberlain",
                    "ItemType": "Users"
                }
            },
            "Content": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Maecenas nec bibendum ex, vel venenatis tortor. Donec elementum urna ac faucibus suscipit. Aliquam pulvinar augue venenatis euismod malesuada.",
            "IsPublished": true,
            "CommentBy": {
                "Id": "fbc2a562-90fe-4765-bf8a-80859fdeb916",
                "DisplayTitle": "Comment By",
                "ItemDisplayText": "Aaron Davidson",
                "ItemType": "People"
            }
        },
    ],
    "EntityDetails": {
        "DateCreated": "2022-07-19T13:05:48.4378498+00:00",
        "DateModified": "2022-07-19T13:05:48.4378498+00:00",
        "CreatedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "FeedbackActionType": {
        "Id": "0ca6f019-d8ec-46c1-b2fe-19a1e2a2d709",
        "DisplayTitle": "FeedBack Action Type",
        "ItemDisplayText": "General Feedback",
        "ItemType": "LookupListEntries"
    },
    "Content": "Morbi laoreet velit id vestibulum vehicula. Proin nec felis eget velit aliquam malesuada nec et sapien. Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
    "IsPublished": false,
    "IsInternal": true,
    "FeedbackActionDate": "2016-01-01T00:01:01.001+00:00",
    "TargetFeedback": [
        {
            "Id": "4f90a43c-4fcb-495d-a3f9-001130523ce7",
            "ItemDisplayText": "Mr Ezra Forster",
            "ItemType": "People"
        }
    ],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "b6a53fe4-8fbd-4393-b718-af57a92e896c",
            "ItemDisplayText": "Glen Ronald Chamberlain",
            "ItemType": "People"
        }
    ]
}
```

Retrieve information for a specific `Feedback` journal entity from the database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/feedback/{id}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Feedback` journal entity you wish to view.

## GET /api/v1/feedback/{id}/comments/{commentId}
> Example (cURL)
```shell
curl --location 'https://{subdomain}.invenias.com/api/v1/feedback/d5344478-4f69-4c42-ba61-f1b99b8565d0/comments/29edd66d-5ccd-44d8-9a3b-c7b2a15a906f' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
{
    "Id": "29edd66d-5ccd-44d8-9a3b-c7b2a15a906f",
    "EntityDetails": {
        "DateCreated": "2023-07-31T13:22:20.9680461+01:00",
        "DateModified": "2023-07-31T13:22:20.9680461+01:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Content": "Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
    "IsPublished": false,
    "CommentBy": {
        "Id": "a9504f72-4f20-4d4a-a8d1-1a18ed571c9d",
        "DisplayTitle": "Comment By",
        "ItemDisplayText": "Glen Chamberlain",
        "ItemType": "People"
    }
}
```

Retrieve a collection of `Comment` entities associated with a specific `Feedback` journal entity in the database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/feedback/{id}/comments/{commentId}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Feedback` journal entity.
commentId | [required] | Please provide the specific unique identifier for the desired `Feedback` comment you wish to retrieve.

## PUT /api/v1/feedback/{id}
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/feedback/5db66c7c-58ea-4f99-8a2d-fd48173d8815' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data '{
  "FeedbackActionType": {
    "Id": "0CA6F019-D8EC-46C1-B2FE-19A1E2A2D709"
  },
  "Content": "Cras feugiat tempus quam eget semper. Quisque ac eros odio. Duis at venenatis ipsum. Sed iaculis rutrum nulla, quis interdum ante dictum ut. Mauris non nisi viverra, ullamcorper nulla vitae, aliquet velit. Nam scelerisque, nibh vel imperdiet sagittis, massa erat euismod est, a mattis leo lectus eu tellus. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.",
  "IsPublished": false,
  "IsInternal": false,
  "FeedbackActionDate": "2017-03-13T17:50:39.93+00:00",
  "TargetFeedback": {
    "ItemReferences": [
      {
        "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd"
      }
    ]
  },
  "IsPinned": false,
  "IsConfidential": false,
  "People": {
    "ItemReferences": [
      {
        "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd"
      }
    ]
  }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "5db66c7c-58ea-4f99-8a2d-fd48173d8815",
    "FeedbackComments": [
        {
            "Id": "29edd66d-5ccd-44d8-9a3b-c7b2a15a906f",
            "EntityDetails": {
                "DateCreated": "2023-07-31T13:22:20.9680461+01:00",
                "DateModified": "2023-07-31T13:22:20.9680461+01:00",
                "CreatedBy": {
                    "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
                    "ItemDisplayText": "Glen Chamberlain",
                    "ItemType": "Users"
                },
                "ModifiedBy": {
                    "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
                    "ItemDisplayText": "Glen Chamberlain",
                    "ItemType": "Users"
                },
                "Owner": {
                    "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
                    "ItemDisplayText": "Glen Chamberlain",
                    "ItemType": "Users"
                }
            },
            "Content": "Test Comment",
            "IsPublished": false,
            "CommentBy": {
                "Id": "a9504f72-4f20-4d4a-a8d1-1a18ed571c9d",
                "DisplayTitle": "Comment By",
                "ItemDisplayText": "Glen Chamberlain",
                "ItemType": "People"
            }
        }
    ],
    "EntityDetails": {
        "DateCreated": "2017-03-13T17:50:39.93+00:00",
        "DateModified": "2023-07-31T13:08:11.6149048+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "FeedbackActionType": {
        "Id": "0ca6f019-d8ec-46c1-b2fe-19a1e2a2d709",
        "DisplayTitle": "FeedBack Action Type",
        "ItemDisplayText": "General Feedback",
        "ItemType": "LookupListEntries"
    },
    "Content": "Cras feugiat tempus quam eget semper. Quisque ac eros odio. Duis at venenatis ipsum. Sed iaculis rutrum nulla, quis interdum ante dictum ut. Mauris non nisi viverra, ullamcorper nulla vitae, aliquet velit. Nam scelerisque, nibh vel imperdiet sagittis, massa erat euismod est, a mattis leo lectus eu tellus. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.",
    "IsPublished": false,
    "IsInternal": false,
    "FeedbackActionDate": "2017-03-13T17:50:39.93+00:00",
    "Programmes": [],
    "TargetFeedback": [
        {
            "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd",
            "ItemDisplayText": "Annabella Rose Thomas (Anna)",
            "ItemType": "People"
        }
    ],
    "Appointments": [],
    "Emails": [],
    "Interviews": [],
    "Notes": [],
    "SentCurriculumVitaes": [],
    "SentDocuments": [],
    "Tasks": [],
    "Telephones": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd",
            "ItemDisplayText": "Annabella Rose Thomas (Anna)",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": []
}
```

Enables the substitution of the desired `Feedback` journal entity representation with the data from the request payload.

### HTTP Request
`https://{{subdomain}}.invenias.com/api/v1/feedback/5db66c7c-58ea-4f99-8a2d-fd48173d8815`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Please provide the unique identifier for the desired `Feedback` journal entity that you wish to replace.

## PUT /api/v1/feedback/{id}/comments/{commentId}
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/feedback/5db66c7c-58ea-4f99-8a2d-fd48173d8815/comments/29edd66d-5ccd-44d8-9a3b-c7b2a15a906f' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data '{
  "Content": "Integer et viverra felis. Donec sit amet magna enim. Maecenas efficitur facilisis neque, et pellentesque ante varius non. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Donec aliquet enim nisi, at tempor mi porta id. Sed lobortis odio at libero suscipit, nec imperdiet massa congue. Nulla neque diam, condimentum nec magna et, tincidunt interdum felis. Nunc urna felis, fermentum quis accumsan sit amet, sollicitudin in est. Phasellus dignissim, orci id tincidunt varius, odio leo pulvinar neque, sit amet suscipit turpis dui in tortor.",
  "IsPublished": true,
  "CommentBy": {
    "Id": "a9504f72-4f20-4d4a-a8d1-1a18ed571c9d"
  }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "29edd66d-5ccd-44d8-9a3b-c7b2a15a906f",
    "EntityDetails": {
        "DateCreated": "2023-07-31T13:22:20.9680461+01:00",
        "DateModified": "2023-07-31T13:20:05.1257742+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Content": "Integer et viverra felis. Donec sit amet magna enim. Maecenas efficitur facilisis neque, et pellentesque ante varius non. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Donec aliquet enim nisi, at tempor mi porta id. Sed lobortis odio at libero suscipit, nec imperdiet massa congue. Nulla neque diam, condimentum nec magna et, tincidunt interdum felis. Nunc urna felis, fermentum quis accumsan sit amet, sollicitudin in est. Phasellus dignissim, orci id tincidunt varius, odio leo pulvinar neque, sit amet suscipit turpis dui in tortor.",
    "IsPublished": true,
    "CommentBy": {
        "Id": "a9504f72-4f20-4d4a-a8d1-1a18ed571c9d",
        "DisplayTitle": "Comment By",
        "ItemDisplayText": "Glen Chamberlain",
        "ItemType": "People"
    }
}
```

Permits the replacement of the existing representation of a `Comment` journal entity, which is relationally associated with any chosen `Feedback` entity in the database, using the data from the request payload.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/feedback/5db66c7c-58ea-4f99-8a2d-fd48173d8815/comments/29edd66d-5ccd-44d8-9a3b-c7b2a15a906f`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Feedback` journal entity.
commentId | [required] | Please provide the specific unique identifier for the desired `Feedback` comment you wish to replace.

## DELETE /api/v1/feedback/{id}
> Example (cURL)
```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/feedback/5db66c7c-58ea-4f99-8a2d-fd48173d8815' \
--header 'Authorization: Bearer {token}'
```

> Please note, successful requests will return a 204 No Content response code.

Enables the permanent removal of any selected `Feedback` journal entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/feedback/5db66c7c-58ea-4f99-8a2d-fd48173d8815`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Feedback` journal entity.

## DELETE /api/v1/feedback/{id}/comments/{commentId}
> Example (cURL)
```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/feedback/97e0aed9-056f-4c35-8aec-a134a0a4340e/comments/20def46e-5ce0-4dae-b071-5a6de453033b' \
--header 'Authorization: Bearer {token}'
```

> Please note, successful requests will return a 204 No Content response code.

Facilitates the irreversible deletion of any `Comment` journal entity that has a relational link to a specific `Feedback` entity in the database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/feedback/97e0aed9-056f-4c35-8aec-a134a0a4340e/comments/20def46e-5ce0-4dae-b071-5a6de453033b`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Feedback` journal entity.
commentId | [required] | To permanently delete the desired `Feedback` comment, please provide the specific unique identifier associated with it.

## POST /api/v1/interviews
> Example (cURL)
```shell
curl --location 'https://{subdomain}.invenias.com/api/v1/interviews' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data '{
  "Subject": "Interview - Head of Sales & Marketing",
  "ClientSubject": "Interview with Annabella Thomas",
  "CandidateSubject": "Interview with Janet Davis",
  "InterviewerName": "Janet Davis",
  "StartDate": "2023-08-01T13:00:04.188Z",
  "EndDate": "2023-08-01T13:30:04.188Z",
  "Location": "Carluccio'\''s, Unit 1, Davidson House, Reading RG1 3EY",
  "IsClientInterview": true,
  "IsConsultantInterview": false,
  "InterviewActionDate": "2023-07-31T13:03:04.188Z",
  "InterviewActionType": {
    "Id": "e5027095-b87c-4de0-94b0-26a24d6885d8",
  },
  "Candidates": {
    "ItemReferences": [
      {
        "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd"
      }
    ]
  },
  "Clients": {
    "ItemReferences": [
      {
        "Id": "e3617a59-2cd1-4eea-974d-aaa9ad4742ab"
      }
    ]
  },
  "IsPinned": false,
  "IsConfidential": false,
  "People": {
    "ItemReferences": [
      {
        "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd"
      },
            {
        "Id": "e3617a59-2cd1-4eea-974d-aaa9ad4742ab"
      }
    ]
  },
  "Assignments": {
    "ItemReferences": [
      {
        "Id": "3f5007c4-3a76-42c0-af75-21e05a7fdeed"
      }
    ]
  },
  "Owner": {
    "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5"
  }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "b6e3c189-89bb-4f36-8dd6-ba55ad4c6d68",
    "EntityDetails": {
        "DateCreated": "2023-07-31T14:01:47.0465882+00:00",
        "DateModified": "2023-07-31T14:01:47.0465882+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Interview - Head of Sales & Marketing",
    "ClientSubject": "Interview with Annabella Thomas",
    "CandidateSubject": "Interview with Janet Davis",
    "InterviewerName": "Janet Davis",
    "StartDate": "2023-08-01T13:00:04.188+00:00",
    "EndDate": "2023-08-01T13:30:04.188+00:00",
    "Location": "Carluccio's, Unit 1, Davidson House, Reading RG1 3EY",
    "IsClientInterview": true,
    "IsConsultantInterview": false,
    "InterviewActionDate": "2023-07-31T13:03:04.188+00:00",
    "InterviewActionType": {
        "Id": "e5027095-b87c-4de0-94b0-26a24d6885d8",
        "DisplayTitle": "Interview Action Type",
        "ItemDisplayText": "1st Interview",
        "ItemType": "LookupListEntries"
    },
    "IsPrivate": false,
    "Appointments": [],
    "Candidates": [
        {
            "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd",
            "ItemDisplayText": "Annabella Rose Thomas (Anna)",
            "ItemType": "People"
        }
    ],
    "Clients": [
        {
            "Id": "e3617a59-2cd1-4eea-974d-aaa9ad4742ab",
            "ItemDisplayText": "Janet Davis",
            "ItemType": "People"
        }
    ],
    "Consultants": [],
    "CandidateEmails": [],
    "CandidateSentDocuments": [],
    "ClientEmails": [],
    "ClientSentDocuments": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd",
            "ItemDisplayText": "Annabella Rose Thomas (Anna)",
            "ItemType": "People"
        },
        {
            "Id": "e3617a59-2cd1-4eea-974d-aaa9ad4742ab",
            "ItemDisplayText": "Janet Davis",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": [
        {
            "Id": "3f5007c4-3a76-42c0-af75-21e05a7fdeed",
            "ItemDisplayText": "Head of Sales & Marketing",
            "ItemType": "Assignments"
        }
    ]
}
```

This endpoint is specifically designed for creating the `Interview` journal entity.

<aside class="notice">
    It is important to note that this method cannot be used to send interview invitations via email, nor can it add the interview to end-users' calendars. Its sole purpose is to create historical data, capturing relevant details for record-keeping and reference.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/interviews`

## GET /api/v1/interviews/{id}
> Example (cURL)
```shell
curl --location 'https://{subdomain}.invenias.com/api/v1/interviews/b6e3c189-89bb-4f36-8dd6-ba55ad4c6d68' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
{
    "Id": "b6e3c189-89bb-4f36-8dd6-ba55ad4c6d68",
    "EntityDetails": {
        "DateCreated": "2023-07-31T14:01:47.0465882+00:00",
        "DateModified": "2023-07-31T14:01:47.0465882+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Interview - Head of Sales & Marketing",
    "ClientSubject": "Interview with Annabella Thomas",
    "CandidateSubject": "Interview with Janet Davis",
    "InterviewerName": "Janet Davis",
    "StartDate": "2023-08-01T13:00:04.188+00:00",
    "EndDate": "2023-08-01T13:30:04.188+00:00",
    "Location": "Carluccio's, Unit 1, Davidson House, Reading RG1 3EY",
    "IsClientInterview": true,
    "IsConsultantInterview": false,
    "InterviewActionDate": "2023-07-31T13:03:04.188+00:00",
    "InterviewActionType": {
        "Id": "e5027095-b87c-4de0-94b0-26a24d6885d8",
        "DisplayTitle": "Interview Action Type",
        "ItemDisplayText": "1st Interview",
        "ItemType": "LookupListEntries"
    },
    "IsPrivate": false,
    "Appointments": [],
    "Candidates": [
        {
            "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd",
            "ItemDisplayText": "Annabella Rose Thomas (Anna)",
            "ItemType": "People"
        }
    ],
    "Clients": [
        {
            "Id": "e3617a59-2cd1-4eea-974d-aaa9ad4742ab",
            "ItemDisplayText": "Janet Davis",
            "ItemType": "People"
        }
    ],
    "Consultants": [],
    "CandidateEmails": [],
    "CandidateSentDocuments": [],
    "ClientEmails": [],
    "ClientSentDocuments": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd",
            "ItemDisplayText": "Annabella Rose Thomas (Anna)",
            "ItemType": "People"
        },
        {
            "Id": "e3617a59-2cd1-4eea-974d-aaa9ad4742ab",
            "ItemDisplayText": "Janet Davis",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": [
        {
            "Id": "3f5007c4-3a76-42c0-af75-21e05a7fdeed",
            "ItemDisplayText": "Head of Sales & Marketing",
            "ItemType": "Assignments"
        }
    ]
}
```

Retrieve information for a specified `Interview` journal entity from the database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/interviews/b6e3c189-89bb-4f36-8dd6-ba55ad4c6d68`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Interview` journal entity.

## PUT /api/v1/interviews/{id}
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/interviews/b6e3c189-89bb-4f36-8dd6-ba55ad4c6d68' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data '{
  "Subject": "Interview - Head of Sales & Marketing",
  "ClientSubject": "Interview with Annabella Thomas",
  "CandidateSubject": "Interview with Janet Davis",
  "InterviewerName": "Janet Davis",
  "StartDate": "2023-08-01T13:00:04.188Z",
  "EndDate": "2023-08-01T13:30:04.188Z",
  "Location": "Starbucks, Buttermarket, Reading RG1 2DE",
  "IsClientInterview": true,
  "IsConsultantInterview": false,
  "InterviewActionDate": "2023-07-31T13:03:04.188Z",
  "InterviewActionType": {
    "Id": "e5027095-b87c-4de0-94b0-26a24d6885d8",
  },
  "Candidates": {
    "ItemReferences": [
      {
        "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd"
      }
    ]
  },
  "Clients": {
    "ItemReferences": [
      {
        "Id": "e3617a59-2cd1-4eea-974d-aaa9ad4742ab"
      }
    ]
  },
  "IsPinned": false,
  "IsConfidential": false,
  "People": {
    "ItemReferences": [
      {
        "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd"
      },
            {
        "Id": "e3617a59-2cd1-4eea-974d-aaa9ad4742ab"
      }
    ]
  },
  "Assignments": {
    "ItemReferences": [
      {
        "Id": "3f5007c4-3a76-42c0-af75-21e05a7fdeed"
      }
    ]
  },
  "Owner": {
    "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5"
  }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "b6e3c189-89bb-4f36-8dd6-ba55ad4c6d68",
    "EntityDetails": {
        "DateCreated": "2023-07-31T14:01:47.0465882+00:00",
        "DateModified": "2023-07-31T15:21:38.6673958+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Interview - Head of Sales & Marketing",
    "ClientSubject": "Interview with Annabella Thomas",
    "CandidateSubject": "Interview with Janet Davis",
    "InterviewerName": "Janet Davis",
    "StartDate": "2023-08-01T13:00:04.188+00:00",
    "EndDate": "2023-08-01T13:30:04.188+00:00",
    "Location": "Starbucks, Buttermarket, Reading RG1 2DE",
    "IsClientInterview": true,
    "IsConsultantInterview": false,
    "InterviewActionDate": "2023-07-31T13:03:04.188+00:00",
    "InterviewActionType": {
        "Id": "e5027095-b87c-4de0-94b0-26a24d6885d8",
        "DisplayTitle": "Interview Action Type",
        "ItemDisplayText": "1st Interview",
        "ItemType": "LookupListEntries"
    },
    "IsPrivate": false,
    "Appointments": [],
    "Candidates": [
        {
            "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd",
            "ItemDisplayText": "Annabella Rose Thomas (Anna)",
            "ItemType": "People"
        }
    ],
    "Clients": [
        {
            "Id": "e3617a59-2cd1-4eea-974d-aaa9ad4742ab",
            "ItemDisplayText": "Janet Davis",
            "ItemType": "People"
        }
    ],
    "Consultants": [],
    "CandidateEmails": [],
    "CandidateSentDocuments": [],
    "ClientEmails": [],
    "ClientSentDocuments": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd",
            "ItemDisplayText": "Annabella Rose Thomas (Anna)",
            "ItemType": "People"
        },
        {
            "Id": "e3617a59-2cd1-4eea-974d-aaa9ad4742ab",
            "ItemDisplayText": "Janet Davis",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": [
        {
            "Id": "3f5007c4-3a76-42c0-af75-21e05a7fdeed",
            "ItemDisplayText": "Head of Sales & Marketing",
            "ItemType": "Assignments"
        }
    ]
}
```

Enables the substitution of the current `Interview` journal entity representation with the data provided in the request payload.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/interviews/b6e3c189-89bb-4f36-8dd6-ba55ad4c6d68`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Interview` journal entity.

## DELETE /api/v1/interviews/{id}
> Example (cURL)
```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/interviews/b6e3c189-89bb-4f36-8dd6-ba55ad4c6d68' \
--header 'Authorization: Bearer {token}'
```

> Please note, successful requests will return a 200 OK response code.

Facilitates the permanent removal of any selected `Interview` journal entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/interviews/b6e3c189-89bb-4f36-8dd6-ba55ad4c6d68`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Interview` journal entity.

## POST /api/v1/notes
> Example (cURL)
```shell
curl --location 'https://{subdomain}.invenias.com/api/v1/notes' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data '{
    "Subject": "Lorem ipsum dolor sit amet, consectetur adipiscing elit",
    "Content": "Aenean ac diam sodales, ultrices nisl sed, iaculis nisi. Mauris malesuada, justo ut laoreet malesuada, mi sem feugiat enim, non eleifend nunc ex ac odio. Mauris et erat a mi accumsan molestie sed nec quam. Donec ultrices aliquet lorem in imperdiet. Curabitur feugiat scelerisque erat.",
    "NoteActionDate": "2022-05-20T15:11:43.052Z",
    "NoteActionType": {
        "Id": "c0f5b294-d4f8-4290-a0dc-28a6bd57699d"
    },
    "IsPinned": false,
    "IsConfidential": false,
    "People": {
        "ItemReferences": [
            {
                "Id": "98e1c913-6838-413e-927c-113c0222b91e"
            }
        ]
    },
    "Assignments": {
        "ItemReferences": [
            {
                "Id": "ca866e18-6c8f-47b0-a76a-0dd29d498e6b"
            }
        ]
    }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "e4bf65c6-43cb-4626-b389-1f81eee3f91d",
    "EntityDetails": {
        "DateCreated": "2023-07-31T15:25:17.0560021+00:00",
        "DateModified": "2023-07-31T15:25:17.0560021+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Lorem ipsum dolor sit amet, consectetur adipiscing elit",
    "Content": "Aenean ac diam sodales, ultrices nisl sed, iaculis nisi. Mauris malesuada, justo ut laoreet malesuada, mi sem feugiat enim, non eleifend nunc ex ac odio. Mauris et erat a mi accumsan molestie sed nec quam. Donec ultrices aliquet lorem in imperdiet. Curabitur feugiat scelerisque erat.",
    "NoteActionDate": "2022-05-20T15:11:43.052+00:00",
    "NoteActionType": {
        "Id": "c0f5b294-d4f8-4290-a0dc-28a6bd57699d",
        "DisplayTitle": "Note Action Type",
        "ItemDisplayText": "Applicant - Addtional Notes",
        "ItemType": "LookupListEntries"
    },
    "Appointments": [],
    "Tasks": [],
    "Programmes": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "98e1c913-6838-413e-927c-113c0222b91e",
            "ItemDisplayText": "Jonas Doe",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": [
        {
            "Id": "ca866e18-6c8f-47b0-a76a-0dd29d498e6b",
            "ItemDisplayText": "Senior Associate",
            "ItemType": "Assignments"
        }
    ]
}
```

Employed for the establishment of a `Note` journal entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/note`

## GET /api/v1/notes/{id}
> Example (cURL)
```shell
curl --location 'https://{subdomain}.invenias.com/api/v1/notes/e4bf65c6-43cb-4626-b389-1f81eee3f91d' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
{
    "Id": "e4bf65c6-43cb-4626-b389-1f81eee3f91d",
    "EntityDetails": {
        "DateCreated": "2023-07-31T15:25:17.0560021+00:00",
        "DateModified": "2023-07-31T15:25:17.0560021+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Lorem ipsum dolor sit amet, consectetur adipiscing elit",
    "Content": "Aenean ac diam sodales, ultrices nisl sed, iaculis nisi. Mauris malesuada, justo ut laoreet malesuada, mi sem feugiat enim, non eleifend nunc ex ac odio. Mauris et erat a mi accumsan molestie sed nec quam. Donec ultrices aliquet lorem in imperdiet. Curabitur feugiat scelerisque erat.",
    "NoteActionDate": "2022-05-20T15:11:43.052+00:00",
    "NoteActionType": {
        "Id": "c0f5b294-d4f8-4290-a0dc-28a6bd57699d",
        "DisplayTitle": "Note Action Type",
        "ItemDisplayText": "Applicant - Addtional Notes",
        "ItemType": "LookupListEntries"
    },
    "Appointments": [],
    "Tasks": [],
    "Programmes": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "98e1c913-6838-413e-927c-113c0222b91e",
            "ItemDisplayText": "Jonas Doe",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": [
        {
            "Id": "ca866e18-6c8f-47b0-a76a-0dd29d498e6b",
            "ItemDisplayText": "Senior Associate",
            "ItemType": "Assignments"
        }
    ]
}
```

Retrieve information for a specified `Note` journal entity from the database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/notes/e4bf65c6-43cb-4626-b389-1f81eee3f91d`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Note` journal entity.

## PUT /api/v1/notes/{id}
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/notes/e4bf65c6-43cb-4626-b389-1f81eee3f91d' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data '{
    "Subject": "Lorem ipsum dolor sit amet, consectetur adipiscing elit",
    "Content": "Aenean ac diam sodales, ultrices nisl sed, iaculis nisi. Mauris malesuada, justo ut laoreet malesuada, mi sem feugiat enim, non eleifend nunc ex ac odio. Mauris et erat a mi accumsan molestie sed nec quam. Donec ultrices aliquet lorem in imperdiet. Curabitur feugiat scelerisque erat.",
    "NoteActionDate": "2022-02-21T10:07:43.052Z",
    "IsPinned": false,
    "IsConfidential": false,
    "People": {
        "ItemReferences": [
            {
                "Id": "98e1c913-6838-413e-927c-113c0222b91e"
            }
        ]
    },
    "Owner": {
        "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5"
    }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "e4bf65c6-43cb-4626-b389-1f81eee3f91d",
    "EntityDetails": {
        "DateCreated": "2023-07-31T15:25:17.0560021+00:00",
        "DateModified": "2023-07-31T15:31:35.1381218+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Lorem ipsum dolor sit amet, consectetur adipiscing elit",
    "Content": "Aenean ac diam sodales, ultrices nisl sed, iaculis nisi. Mauris malesuada, justo ut laoreet malesuada, mi sem feugiat enim, non eleifend nunc ex ac odio. Mauris et erat a mi accumsan molestie sed nec quam. Donec ultrices aliquet lorem in imperdiet. Curabitur feugiat scelerisque erat.",
    "NoteActionDate": "2022-02-21T10:07:43.052+00:00",
    "Appointments": [],
    "Tasks": [],
    "Programmes": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "98e1c913-6838-413e-927c-113c0222b91e",
            "ItemDisplayText": "Jonas Doe",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": []
}
```

Enables you to update the representation of the chosen `Note` journal entity using the data from the request payload.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/notes/e4bf65c6-43cb-4626-b389-1f81eee3f91d`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Note` journal entity.

## DELETE /api/v1/notes/{id}
> Example (cURL)
```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/notes/e4bf65c6-43cb-4626-b389-1f81eee3f91d' \
--header 'Authorization: Bearer {token}'
```

> Please note, successful requests will return a 200 OK response code.

Facilitates the irrevocable deletion of a specified `Note` journal entity.

### HTTP Request
`https://{{subdomain}}.invenias.com/api/v1/notes/e4bf65c6-43cb-4626-b389-1f81eee3f91d`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Note` journal entity.

## POST /api/v1/telephones
> Example (cURL)
```shell
curl --location 'https://{subdomain}.invenias.com/api/v1/telephones' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data '{
  "Subject": "Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
  "Content": "Suspendisse eleifend tincidunt velit at pulvinar. Vestibulum malesuada egestas quam, quis rutrum ante feugiat ut. In nunc magna, malesuada eu diam dapibus, ullamcorper semper neque. Integer efficitur sollicitudin aliquam. Quisque odio nulla, consectetur ac ullamcorper sit amet, consequat eu elit. Maecenas eleifend hendrerit interdum. Mauris tincidunt libero sed sem maximus pretium. Nunc efficitur suscipit tortor, non finibus diam volutpat vel.",
  "TelephoneActionDate": "2023-02-21T10:07:43.052Z",
  "TelephoneActionType": {
    "Id": "7588e19f-97e7-4dbd-bdda-5d1da9cdf82e"
  },
  "IsPinned": false,
  "IsConfidential": false,
  "People": {
    "ItemReferences": [
      {
        "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f"
      }
    ]
  },
  "Companies": {
    "ItemReferences": [
      {
        "Id": "c6bd6734-fc2f-4088-bbcd-8c038e549c01"
      }
    ]
  }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "4da3ffd0-a0bc-41b0-bcc8-0ff62e2fbc2d",
    "EntityDetails": {
        "DateCreated": "2023-08-01T10:44:54.665413+00:00",
        "DateModified": "2023-08-01T10:44:54.665413+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
    "Content": "Suspendisse eleifend tincidunt velit at pulvinar. Vestibulum malesuada egestas quam, quis rutrum ante feugiat ut. In nunc magna, malesuada eu diam dapibus, ullamcorper semper neque. Integer efficitur sollicitudin aliquam. Quisque odio nulla, consectetur ac ullamcorper sit amet, consequat eu elit. Maecenas eleifend hendrerit interdum. Mauris tincidunt libero sed sem maximus pretium. Nunc efficitur suscipit tortor, non finibus diam volutpat vel.",
    "TelephoneActionDate": "2023-02-21T10:07:43.052+00:00",
    "TelephoneActionType": {
        "Id": "7588e19f-97e7-4dbd-bdda-5d1da9cdf82e",
        "DisplayTitle": "Telephone Action Type",
        "ItemDisplayText": "Business Development",
        "ItemType": "LookupListEntries"
    },
    "Appointments": [],
    "Tasks": [],
    "Programmes": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f",
            "ItemDisplayText": "Lori Gray",
            "ItemType": "People"
        }
    ],
    "Companies": [
        {
            "Id": "c6bd6734-fc2f-4088-bbcd-8c038e549c01",
            "ItemDisplayText": "Lloyds Banking Group",
            "ItemType": "Companies"
        }
    ],
    "Assignments": []
}
```

Used to create an `Phone Call` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/telephones`

## GET /api/v1/telephones/{id}
> Example (cURL)
```shell
curl --location 'https://{subdomain}.invenias.com/api/v1/telephones/4da3ffd0-a0bc-41b0-bcc8-0ff62e2fbc2d' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
{
    "Id": "4da3ffd0-a0bc-41b0-bcc8-0ff62e2fbc2d",
    "EntityDetails": {
        "DateCreated": "2023-08-01T10:44:54.665413+00:00",
        "DateModified": "2023-08-01T10:44:54.665413+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
    "Content": "Suspendisse eleifend tincidunt velit at pulvinar. Vestibulum malesuada egestas quam, quis rutrum ante feugiat ut. In nunc magna, malesuada eu diam dapibus, ullamcorper semper neque. Integer efficitur sollicitudin aliquam. Quisque odio nulla, consectetur ac ullamcorper sit amet, consequat eu elit. Maecenas eleifend hendrerit interdum. Mauris tincidunt libero sed sem maximus pretium. Nunc efficitur suscipit tortor, non finibus diam volutpat vel.",
    "TelephoneActionDate": "2023-02-21T10:07:43.052+00:00",
    "TelephoneActionType": {
        "Id": "7588e19f-97e7-4dbd-bdda-5d1da9cdf82e",
        "DisplayTitle": "Telephone Action Type",
        "ItemDisplayText": "Business Development",
        "ItemType": "LookupListEntries"
    },
    "Appointments": [],
    "Tasks": [],
    "Programmes": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f",
            "ItemDisplayText": "Lori Gray",
            "ItemType": "People"
        }
    ],
    "Companies": [
        {
            "Id": "c6bd6734-fc2f-4088-bbcd-8c038e549c01",
            "ItemDisplayText": "Lloyds Banking Group",
            "ItemType": "Companies"
        }
    ],
    "Assignments": []
}
```

Utilized for the initiation of a `Phone Call` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/telephones/4da3ffd0-a0bc-41b0-bcc8-0ff62e2fbc2d`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Telephone` journal entity.

## PUT /api/v1/telephones/{id}
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/telephones/4da3ffd0-a0bc-41b0-bcc8-0ff62e2fbc2d' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data '{
  "Subject": "Lorem ipsum dolor sit amet, consectetur adipiscing elit",
  "Content": "Suspendisse eleifend tincidunt velit at pulvinar. Vestibulum malesuada egestas quam, quis rutrum ante feugiat ut. In nunc magna, malesuada eu diam dapibus, ullamcorper semper neque. Integer efficitur sollicitudin aliquam. Quisque odio nulla, consectetur ac ullamcorper sit amet, consequat eu elit. Maecenas eleifend hendrerit interdum. Mauris tincidunt libero sed sem maximus pretium. Nunc efficitur suscipit tortor, non finibus diam volutpat vel.",
  "TelephoneActionDate": "2023-02-21T10:07:43.052Z",
  "TelephoneActionType": {
    "Id": "7588e19f-97e7-4dbd-bdda-5d1da9cdf82e"
  },
  "IsPinned": false,
  "IsConfidential": false,
  "People": {
    "ItemReferences": [
      {
        "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f"
      }
    ]
  },
  "Companies": {
    "ItemReferences": []
  }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "4da3ffd0-a0bc-41b0-bcc8-0ff62e2fbc2d",
    "EntityDetails": {
        "DateCreated": "2023-08-01T10:44:54.665413+00:00",
        "DateModified": "2023-08-01T10:51:26.5163466+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Lorem ipsum dolor sit amet, consectetur adipiscing elit",
    "Content": "Suspendisse eleifend tincidunt velit at pulvinar. Vestibulum malesuada egestas quam, quis rutrum ante feugiat ut. In nunc magna, malesuada eu diam dapibus, ullamcorper semper neque. Integer efficitur sollicitudin aliquam. Quisque odio nulla, consectetur ac ullamcorper sit amet, consequat eu elit. Maecenas eleifend hendrerit interdum. Mauris tincidunt libero sed sem maximus pretium. Nunc efficitur suscipit tortor, non finibus diam volutpat vel.",
    "TelephoneActionDate": "2023-02-21T10:07:43.052+00:00",
    "TelephoneActionType": {
        "Id": "7588e19f-97e7-4dbd-bdda-5d1da9cdf82e",
        "DisplayTitle": "Telephone Action Type",
        "ItemDisplayText": "Business Development",
        "ItemType": "LookupListEntries"
    },
    "Appointments": [],
    "Tasks": [],
    "Programmes": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f",
            "ItemDisplayText": "Lori Gray",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": []
}
```

Enables the replacement of the chosen `Phone Call` journal entity's representation with the data contained in the request payload.

### HTTP Request
`https://{{subdomain}}.invenias.com/api/v1/telephones/4da3ffd0-a0bc-41b0-bcc8-0ff62e2fbc2d`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Telephone` journal entity.


## DELETE /api/v1/telephones/{id}
> Example (cURL)
```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/telephones/4da3ffd0-a0bc-41b0-bcc8-0ff62e2fbc2d' \
--header 'Authorization: Bearer {token}'
```

> Please note, successful requests will return a 200 OK response code.

Facilitates the irreversible removal of any specified `Phone Call` journal entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/telephones/4da3ffd0-a0bc-41b0-bcc8-0ff62e2fbc2d`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Telephone` journal entity.

## POST /api/v1/sentcurriculumvitaes
> Example (cURL)
```shell
curl --location 'https://{subdomain}.invenias.com/api/v1/sentcurriculumvitaes' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data '{
    "Subject": "Nam imperdiet et enim ut eleifend.",
    "IsSent": true,
    "SentOn": "2023-07-31T13:03:07.982Z",
    "ReceivedTime": "2023-07-31T13:03:07.982Z",
    "CvActionDate": "2023-07-31T13:03:07.982Z",
    "SentCurriculumVitaeActionType": {
        "Id": "6b222d8e-3c21-4cb7-8eb0-b3d7493679e2"
    },
    "AddressedToPeople": {
        "ItemReferences": [
            {
                "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f"
            }
        ]
    },
    "Candidates": {
        "ItemReferences": [
            {
                "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd"
            }
        ]
    },
    "Clients": {
        "ItemReferences": [
            {
                "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f"
            }
        ]
    },
    "IsPinned": false,
    "IsConfidential": false,
    "People": {
        "ItemReferences": [
            {
                "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f"
            },
            {
                "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd"
            }
        ]
    },
    "Assignments": {
        "ItemReferences": [
            {
                "Id": "3f5007c4-3a76-42c0-af75-21e05a7fdeed"
            }
        ]
    }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "b1065c18-3ea0-4327-a639-d3e223454059",
    "EntityDetails": {
        "DateCreated": "2023-08-01T12:11:57.7271332+00:00",
        "DateModified": "2023-08-01T12:11:57.7271332+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Nam imperdiet et enim ut eleifend.",
    "IsSent": true,
    "SentOn": "2023-07-31T13:03:07.982+00:00",
    "ReceivedTime": "2023-07-31T13:03:07.982+00:00",
    "CvActionDate": "2023-07-31T13:03:07.982+00:00",
    "SentCurriculumVitaeActionType": {
        "Id": "6b222d8e-3c21-4cb7-8eb0-b3d7493679e2",
        "DisplayTitle": "Sent Curriculum Vitae Action Type",
        "ItemDisplayText": "CV/Resume Submitted",
        "ItemType": "LookupListEntries"
    },
    "AddressedToPeople": [
        {
            "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f",
            "ItemDisplayText": "Lori Gray",
            "ItemType": "People"
        }
    ],
    "AddressedToCompanies": [],
    "Candidates": [
        {
            "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd",
            "ItemDisplayText": "Annabella Rose Thomas (Anna)",
            "ItemType": "People"
        }
    ],
    "Clients": [
        {
            "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f",
            "ItemDisplayText": "Lori Gray",
            "ItemType": "People"
        }
    ],
    "Documents": [],
    "SentDocuments": [],
    "ClientEmails": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd",
            "ItemDisplayText": "Annabella Rose Thomas (Anna)",
            "ItemType": "People"
        },
        {
            "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f",
            "ItemDisplayText": "Lori Gray",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": [
        {
            "Id": "3f5007c4-3a76-42c0-af75-21e05a7fdeed",
            "ItemDisplayText": "Head of Sales & Marketing",
            "ItemType": "Assignments"
        }
    ]
}
```

Employed for the creation of a `Send CV` journal entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/sentcurriculumvitaes`

## GET /api/v1/sentcurriculumvitaes/{id}
> Example (cURL)
```shell
curl --location 'https://{subdomain}.invenias.com/api/v1/sentcurriculumvitaes/b1065c18-3ea0-4327-a639-d3e223454059' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
{
    "Id": "b1065c18-3ea0-4327-a639-d3e223454059",
    "EntityDetails": {
        "DateCreated": "2023-08-01T12:11:57.7271332+00:00",
        "DateModified": "2023-08-01T12:11:57.7271332+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Nam imperdiet et enim ut eleifend.",
    "IsSent": true,
    "SentOn": "2023-07-31T13:03:07.982+00:00",
    "ReceivedTime": "2023-07-31T13:03:07.982+00:00",
    "CvActionDate": "2023-07-31T13:03:07.982+00:00",
    "SentCurriculumVitaeActionType": {
        "Id": "6b222d8e-3c21-4cb7-8eb0-b3d7493679e2",
        "DisplayTitle": "Sent Curriculum Vitae Action Type",
        "ItemDisplayText": "CV/Resume Submitted",
        "ItemType": "LookupListEntries"
    },
    "AddressedToPeople": [
        {
            "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f",
            "ItemDisplayText": "Lori Gray",
            "ItemType": "People"
        }
    ],
    "AddressedToCompanies": [],
    "Candidates": [
        {
            "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd",
            "ItemDisplayText": "Annabella Rose Thomas (Anna)",
            "ItemType": "People"
        }
    ],
    "Clients": [
        {
            "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f",
            "ItemDisplayText": "Lori Gray",
            "ItemType": "People"
        }
    ],
    "Documents": [],
    "SentDocuments": [],
    "ClientEmails": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd",
            "ItemDisplayText": "Annabella Rose Thomas (Anna)",
            "ItemType": "People"
        },
        {
            "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f",
            "ItemDisplayText": "Lori Gray",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": [
        {
            "Id": "3f5007c4-3a76-42c0-af75-21e05a7fdeed",
            "ItemDisplayText": "Head of Sales & Marketing",
            "ItemType": "Assignments"
        }
    ]
}
```

Retrieve information for a specified `Send CV` journal entity from the database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/sentcurriculumvitaes/b1065c18-3ea0-4327-a639-d3e223454059`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Send CV` journal entity.

## PUT /api/v1/sentcurriculumvitaes/{id}
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/sentcurriculumvitaes/b1065c18-3ea0-4327-a639-d3e223454059' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data '{
    "Subject": "Nam imperdiet et enim ut eleifend.",
    "IsSent": true,
    "SentOn": "2023-08-31T13:03:07.982Z",
    "ReceivedTime": "2023-08-31T13:03:07.982Z",
    "CvActionDate": "2023-8-31T13:03:07.982Z",
    "SentCurriculumVitaeActionType": {
        "Id": "6b222d8e-3c21-4cb7-8eb0-b3d7493679e2"
    },
    "AddressedToPeople": {
        "ItemReferences": [
            {
                "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f"
            }
        ]
    },
    "Candidates": {
        "ItemReferences": [
            {
                "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd"
            }
        ]
    },
    "Clients": {
        "ItemReferences": [
            {
                "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f"
            }
        ]
    },
    "IsPinned": false,
    "IsConfidential": false,
    "People": {
        "ItemReferences": [
            {
                "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f"
            },
            {
                "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd"
            }
        ]
    },
    "Assignments": {
        "ItemReferences": [
            {
                "Id": "3f5007c4-3a76-42c0-af75-21e05a7fdeed"
            }
        ]
    }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "b1065c18-3ea0-4327-a639-d3e223454059",
    "EntityDetails": {
        "DateCreated": "2023-08-01T12:11:57.7271332+00:00",
        "DateModified": "2023-08-01T12:26:20.0821966+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Nam imperdiet et enim ut eleifend.",
    "IsSent": true,
    "SentOn": "2023-08-31T13:03:07.982+00:00",
    "ReceivedTime": "2023-08-31T13:03:07.982+00:00",
    "CvActionDate": "2023-08-31T13:03:07.982+00:00",
    "SentCurriculumVitaeActionType": {
        "Id": "6b222d8e-3c21-4cb7-8eb0-b3d7493679e2",
        "DisplayTitle": "Sent Curriculum Vitae Action Type",
        "ItemDisplayText": "CV/Resume Submitted",
        "ItemType": "LookupListEntries"
    },
    "AddressedToPeople": [
        {
            "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f",
            "ItemDisplayText": "Lori Gray",
            "ItemType": "People"
        }
    ],
    "AddressedToCompanies": [],
    "Candidates": [
        {
            "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd",
            "ItemDisplayText": "Annabella Rose Thomas (Anna)",
            "ItemType": "People"
        }
    ],
    "Clients": [
        {
            "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f",
            "ItemDisplayText": "Lori Gray",
            "ItemType": "People"
        }
    ],
    "Documents": [],
    "SentDocuments": [],
    "ClientEmails": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "eb4cb3e4-baee-40ba-b312-847694a685bd",
            "ItemDisplayText": "Annabella Rose Thomas (Anna)",
            "ItemType": "People"
        },
        {
            "Id": "b7cfba62-6c29-4f82-b497-d998ae27911f",
            "ItemDisplayText": "Lori Gray",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": [
        {
            "Id": "3f5007c4-3a76-42c0-af75-21e05a7fdeed",
            "ItemDisplayText": "Head of Sales & Marketing",
            "ItemType": "Assignments"
        }
    ]
}
```

Enables the substitution of the existing `Send CV` journal entity representation with the data provided in the request payload.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/sentcurriculumvitaes/b1065c18-3ea0-4327-a639-d3e223454059`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Send CV` journal entity.


## DELETE /api/v1/sentcurriculumvitaes/{id}
> Example (cURL)
```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/sentcurriculumvitaes/b1065c18-3ea0-4327-a639-d3e223454059' \
--header 'Authorization: Bearer {token}'
```

> Please note, successful requests will return a 200 OK response code.

Facilitates the irrevocable elimination of any selected `Send CV` journal entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/sentcurriculumvitaes/4da3ffd0-a0bc-41b0-bcc8-0ff62e2fbc2d`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Send CV` journal entity.

## POST /api/v1/tasks
> Example (cURL)
```shell
curl --location 'https://{subdomain}.invenias.com/api/v1/tasks' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {token}' \
--data '{
  "Subject": "Etiam rutrum non quam sit amet lacinia.",
  "Content": "Proin hendrerit est eget lectus vehicula facilisis. Nullam vitae enim tempor, aliquam purus in, sodales nisi. Proin felis dui, suscipit non convallis vel, pellentesque a sem. Donec elementum dui sit amet ipsum interdum, non pharetra dui volutpat. Fusce sagittis pellentesque mauris ac vehicula. Curabitur nec commodo ante. Etiam ut lectus sit amet metus fringilla interdum sed non neque. Curabitur lacinia ac dui eget fermentum. Nullam tempus ipsum tempor, iaculis tellus ac, aliquam erat. Donec eu arcu gravida, tempor justo nec, ullamcorper elit.",
  "DueDate": "2023-08-02T08:38:50.636Z",
  "StartDate": "2023-08-02T08:38:50.636Z",
  "Status": 2,
  "Priority": 1,
  "PercentComplete": 100,
  "ReminderTime": "2023-08-02T08:38:50.636Z",
  "DateCompleted": "2023-08-02T08:38:50.636Z",
  "TaskActionDate": "2023-08-02T08:38:50.636Z",
  "HasAttachments": true,
  "TaskActionType": {
    "Id": "14ddf3b5-c8e2-4603-b5c4-e9c78a4786f0",
  },
  "IsPinned": false,
  "IsConfidential": false,
  "People": {
    "ItemReferences": [
      {
        "Id": "98e1c913-6838-413e-927c-113c0222b91e",
      }
    ]
  },
  "Assignments": {
    "ItemReferences": [
      {
        "Id": "ca866e18-6c8f-47b0-a76a-0dd29d498e6b"
      }
    ]
  }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "a1ab1b5f-d421-43d2-a5d4-8c0a7d87cc7b",
    "EntityDetails": {
        "DateCreated": "2023-08-02T08:53:41.9086231+00:00",
        "DateModified": "2023-08-02T08:53:41.9086231+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Etiam rutrum non quam sit amet lacinia.",
    "Content": "Proin hendrerit est eget lectus vehicula facilisis. Nullam vitae enim tempor, aliquam purus in, sodales nisi. Proin felis dui, suscipit non convallis vel, pellentesque a sem. Donec elementum dui sit amet ipsum interdum, non pharetra dui volutpat. Fusce sagittis pellentesque mauris ac vehicula. Curabitur nec commodo ante. Etiam ut lectus sit amet metus fringilla interdum sed non neque. Curabitur lacinia ac dui eget fermentum. Nullam tempus ipsum tempor, iaculis tellus ac, aliquam erat. Donec eu arcu gravida, tempor justo nec, ullamcorper elit.",
    "DueDate": "2023-08-02T08:38:50.636+00:00",
    "StartDate": "2023-08-02T08:38:50.636+00:00",
    "Status": 2,
    "Priority": 1,
    "PercentComplete": 100,
    "ReminderTime": "2023-08-02T08:38:50.636+00:00",
    "DateCompleted": "2023-08-02T08:38:50.636+00:00",
    "TaskActionDate": "2023-08-02T08:38:50.636+00:00",
    "HasAttachments": false,
    "TaskActionType": {
        "Id": "14ddf3b5-c8e2-4603-b5c4-e9c78a4786f0",
        "DisplayTitle": "Task Action Type",
        "ItemDisplayText": "Candidate Call",
        "ItemType": "LookupListEntries"
    },
    "Notes": [],
    "Telephones": [],
    "Emails": [],
    "SentDocuments": [],
    "Programmes": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "98e1c913-6838-413e-927c-113c0222b91e",
            "ItemDisplayText": "Jonas Doe",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": [
        {
            "Id": "ca866e18-6c8f-47b0-a76a-0dd29d498e6b",
            "ItemDisplayText": "Senior Associate",
            "ItemType": "Assignments"
        }
    ]
}
```

Employed to establish a `Task` journal entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/tasks`

## GET /api/v1/tasks/{id}
> Example (cURL)
```shell
curl --location 'https://{subdomain}.invenias.com/api/v1/tasks/a1ab1b5f-d421-43d2-a5d4-8c0a7d87cc7b' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
{
    "Id": "a1ab1b5f-d421-43d2-a5d4-8c0a7d87cc7b",
    "EntityDetails": {
        "DateCreated": "2023-08-02T08:53:41.9086231+00:00",
        "DateModified": "2023-08-02T08:53:41.9086231+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Etiam rutrum non quam sit amet lacinia.",
    "Content": "Proin hendrerit est eget lectus vehicula facilisis. Nullam vitae enim tempor, aliquam purus in, sodales nisi. Proin felis dui, suscipit non convallis vel, pellentesque a sem. Donec elementum dui sit amet ipsum interdum, non pharetra dui volutpat. Fusce sagittis pellentesque mauris ac vehicula. Curabitur nec commodo ante. Etiam ut lectus sit amet metus fringilla interdum sed non neque. Curabitur lacinia ac dui eget fermentum. Nullam tempus ipsum tempor, iaculis tellus ac, aliquam erat. Donec eu arcu gravida, tempor justo nec, ullamcorper elit.",
    "DueDate": "2023-08-02T08:38:50.636+00:00",
    "StartDate": "2023-08-02T08:38:50.636+00:00",
    "Status": 2,
    "Priority": 1,
    "PercentComplete": 100,
    "ReminderTime": "2023-08-02T08:38:50.636+00:00",
    "DateCompleted": "2023-08-02T08:38:50.636+00:00",
    "TaskActionDate": "2023-08-02T08:38:50.636+00:00",
    "HasAttachments": false,
    "TaskActionType": {
        "Id": "14ddf3b5-c8e2-4603-b5c4-e9c78a4786f0",
        "DisplayTitle": "Task Action Type",
        "ItemDisplayText": "Candidate Call",
        "ItemType": "LookupListEntries"
    },
    "Notes": [],
    "Telephones": [],
    "Emails": [],
    "SentDocuments": [],
    "Programmes": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "98e1c913-6838-413e-927c-113c0222b91e",
            "ItemDisplayText": "Jonas Öström",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": [
        {
            "Id": "ca866e18-6c8f-47b0-a76a-0dd29d498e6b",
            "ItemDisplayText": "Senior Associate",
            "ItemType": "Assignments"
        }
    ]
}
```

Retrieve information for a specified `Task` journal entity from the database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/tasks/a1ab1b5f-d421-43d2-a5d4-8c0a7d87cc7b`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Task` journal entity.

## GET /api/v1/tasks/upcoming
> Example (cURL)
```shell
curl --location 'https://{subdomain}.invenias.com/api/v1/tasks/upcoming' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
[
    {
        "CompanyName": "Invenias by Bullhorn",
        "IsOverdue": false,
        "Id": "3a2d5277-b54f-4238-9431-710b626e0a27",
        "DisplayTitle": "Etiam rutrum non quam sit amet lacinia.",
        "DueDateTime": "2023-09-02T08:38:50.636+00:00"
    }
]
```


Retrieve a collection of future-due `Task` journal entities that are scheduled for upcoming dates.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/tasks/upcoming`

## PUT /api/v1/tasks/{id}
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/tasks/a1ab1b5f-d421-43d2-a5d4-8c0a7d87cc7b' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {Token}' \
--data '{
  "Subject": "Nulla pulvinar accumsan nulla.",
  "Content": "Proin hendrerit est eget lectus vehicula facilisis. Nullam vitae enim tempor, aliquam purus in, sodales nisi. Proin felis dui, suscipit non convallis vel, pellentesque a sem. Donec elementum dui sit amet ipsum interdum, non pharetra dui volutpat. Fusce sagittis pellentesque mauris ac vehicula. Curabitur nec commodo ante. Etiam ut lectus sit amet metus fringilla interdum sed non neque. Curabitur lacinia ac dui eget fermentum. Nullam tempus ipsum tempor, iaculis tellus ac, aliquam erat. Donec eu arcu gravida, tempor justo nec, ullamcorper elit.",
  "DueDate": "2023-09-02T08:38:50.636Z",
  "StartDate": "2023-08-10T08:38:50.636Z",
  "Status": 0,
  "Priority": 1,
  "PercentComplete": 50,
  "TaskActionDate": "2023-08-02T08:38:50.636Z",
  "HasAttachments": false,
  "TaskActionType": {
    "Id": "14ddf3b5-c8e2-4603-b5c4-e9c78a4786f0"
  },
  "IsPinned": false,
  "IsConfidential": false,
  "People": {
    "ItemReferences": [
      {
        "Id": "98e1c913-6838-413e-927c-113c0222b91e"
      }
    ]
  },
  "Assignments": {
    "ItemReferences": [
      {
        "Id": "ca866e18-6c8f-47b0-a76a-0dd29d498e6b"
      }
    ]
  }
}'
```

> Example Response (JSON)
```shell
{
    "Id": "a1ab1b5f-d421-43d2-a5d4-8c0a7d87cc7b",
    "EntityDetails": {
        "DateCreated": "2023-08-02T08:53:41.9086231+00:00",
        "DateModified": "2023-08-02T09:14:15.4983034+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "Subject": "Nulla pulvinar accumsan nulla.",
    "Content": "Proin hendrerit est eget lectus vehicula facilisis. Nullam vitae enim tempor, aliquam purus in, sodales nisi. Proin felis dui, suscipit non convallis vel, pellentesque a sem. Donec elementum dui sit amet ipsum interdum, non pharetra dui volutpat. Fusce sagittis pellentesque mauris ac vehicula. Curabitur nec commodo ante. Etiam ut lectus sit amet metus fringilla interdum sed non neque. Curabitur lacinia ac dui eget fermentum. Nullam tempus ipsum tempor, iaculis tellus ac, aliquam erat. Donec eu arcu gravida, tempor justo nec, ullamcorper elit.",
    "DueDate": "2023-09-02T08:38:50.636+00:00",
    "StartDate": "2023-08-10T08:38:50.636+00:00",
    "Status": 0,
    "Priority": 1,
    "PercentComplete": 50,
    "TaskActionDate": "2023-08-02T08:38:50.636+00:00",
    "HasAttachments": false,
    "TaskActionType": {
        "Id": "14ddf3b5-c8e2-4603-b5c4-e9c78a4786f0",
        "DisplayTitle": "Task Action Type",
        "ItemDisplayText": "Candidate Call",
        "ItemType": "LookupListEntries"
    },
    "Notes": [],
    "Telephones": [],
    "Emails": [],
    "SentDocuments": [],
    "Programmes": [],
    "IsPinned": false,
    "IsConfidential": false,
    "People": [
        {
            "Id": "98e1c913-6838-413e-927c-113c0222b91e",
            "ItemDisplayText": "Jonas Öström",
            "ItemType": "People"
        }
    ],
    "Companies": [],
    "Assignments": [
        {
            "Id": "ca866e18-6c8f-47b0-a76a-0dd29d498e6b",
            "ItemDisplayText": "Senior Associate",
            "ItemType": "Assignments"
        }
    ]
}
```

Enables the update of the selected `Task` journal entity representation using the data from the request payload.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/tasks/a1ab1b5f-d421-43d2-a5d4-8c0a7d87cc7b`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Task` journal entity.

## DELETE /api/v1/tasks/{id}
> Example (cURL)
```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/tasks/a1ab1b5f-d421-43d2-a5d4-8c0a7d87cc7b' \
--header 'Authorization: Bearer {Token}'
```

> Please note, successful requests will return a 200 OK response code.

Facilitates the permanent removal of any chosen `Task` journal entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/tasks/a1ab1b5f-d421-43d2-a5d4-8c0a7d87cc7b`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Task` journal entity.


# Companies
<i>Table 1. Company Endpoints Summary</i>

Name | Description
---- | -----------
[POST /api/v1/companies/list] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-list)  | Returns a list of `Company` entities in the database.
[POST /api/v1/companies] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies)  | Creates a `Company` entity in the tenant database.
[GET /api/v1/companies/{id}] (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-companies-id)  | Returns information about any given `Company` entity in the database.
[PUT /api/v2/companies/{id}] (https://bullhorn.github.io/invenias-api-docs/#put-api-v2-companies-id)  | Adds or changes values to any given `Company` entity in the database.
[DELETE /api/v1/companies/{id}] (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-companies-id)  | Deletes any given `Company` entity in the database.
[PUT /api/v1/companies/bulkdelete] (https://bullhorn.github.io/invenias-api-docs/#put-api-v1-companies-bulkdelete)  | Deletes many `Company` entities in the database.
[POST /api/v1/companies/{id}/students/list] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-id-students-list) | Returns a list `People` entities who have one or more `Education` entities relationally linked to a `Company` entity including their related qualifications.
[POST /api/v1/companies/{id}/people/list] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-id-people-list) | Returns a list `People` entities who have one or more `Position` entities relationally linked to a `Company` entity including their employment details.
[POST /api/v1/companies/{id}/bulkremovepeople] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-id-bulkremovepeople) | Deletes the relationship between one or more `People` entities relationally linked to the given `Company` entity and including related `Position` entities.
[POST /api/v1/companies/{id}/currentpeople/list] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-id-currentpeople-list) | Returns a list `People` entities who have one or more `Position` entities where the 'Position Status' is equal to 'Current' that are relationally linked to a `Company` entity including their employment details.
[POST /api/v1/companies/{id}/clientassignments/list] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-id-clientassignments-list) | Returns a list `Assingment` entities relationally linked to the given `Company` entity.
[POST /api/v1/companies/{id}/programmes] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-id-programmes) | Creates a relation between the given `Company` entity and one or more `Programme` entities.
[POST /api/v1/companies/{id}/programmes/list] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-id-programmes-list) | Returns a list of `Programme` entities that are relationally linked to the given `Company` entity.
[GET /api/v1/companies/{id}/programmes/{programmeId}] (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-companies-id-programmes-programmeid) | Returns details of a specific `Programme` entity that is relationally linked to the given `Company` entity.
[DELETE /api/v1/companies/{id}/programmes/{programmeId}] (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-companies-id-programmes-programmeid) | Deletes the relationship between a specific `Programme` entity relationally linked to the given `Company` entity.
[POST /api/v1/companies/{id}/companies] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-id-companies) | Creates a relation between two `Company` entities (e.g. Competitor, Parent, Subsidiary, Investor, Supplier, etc).
[PUT /api/v1/companies/{id}/companies] (https://bullhorn.github.io/invenias-api-docs/#put-api-v1-companies-id-companies) | Adds or changes values to relation between two `Company` entities (e.g. Competitor, Parent, Subsidiary, Investor, Supplier, etc).
[POST /api/v1/companies/{id}/relations/list] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-id-relations-list) | Returns a lists of `Company` entities that are relationally linked to any given `Company` entity including the relationship type.
[POST /api/v1/companies/{id}/bulkremovecompanies] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-id-bulkremovecompanies) | Deletes the relationship between one or more `Company` entities relationally linked to the given `Company` entity.
[POST /api/v1/companies/{id}/externalid1] (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-id-externalid1) | Allows you to add an external unique identifier for the Invenias `Company` entity to be leveraged by an external application where applicable.
[DELETE /api/v1/companies/{id}/externalid1] (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-companies-id-externalid1) | Deletes an external unique identifier for the Invenias `Company` entity which is usually leveraged by an external application for reconciliation purposes.
[GET /api/v1/companies/{id}/notepad] (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-companies-id-notepad) | Returns contents of the 'Notepad' field for the given `Company` entity in either HTML or Plain Text.
[PUT /api/v1/companies/{id}/notepad] (https://bullhorn.github.io/invenias-api-docs/#put-api-v1-companies-id-notepad) | Allows you to populate 'Notepad' field for the given `Company` entity using either HTML or Plain Text.
[GET /api/v1/companies/{id}/profile] (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-companies-id-profile) | Returns contents of the 'Overview' field for the given `Company` entity in either HTML or Plain Text.
[PUT /api/v1/companies/{id}/profile] (https://bullhorn.github.io/invenias-api-docs/#put-api-v1-companies-id-profile) | Allows you to populate 'Overview' field for the given `Company` entity using either HTML or Plain Text.

## POST /api/v1/companies/list
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/companies/list' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "PageSize": 1000,
    "PageIndex": 0,
    "ReturnTotalCount": true,
    "ReturnTotalDatabaseItemCount": true,
    "Select": [
      "FileAs",
      "CompanyReferenceNumber",
      "TotalNumberOfAssignments"
    ],
    "Filter": [
        [
            "TypeClientChecked",
            "=",
            "True"
        ]
    ]
  }'
```

> Example Response (JSON)
```shell
{
    "Items": [
        {
            "CompanyReferenceNumber": "C000054",
            "FileAs": "Lloyds Banking Group",
            "ItemType": "Companies",
            "TotalNumberOfAssignments": 5,
            "TypeClientChecked": true,
            "ItemId": "c6bd6734-fc2f-4088-bbcd-8c038e549c01",
            "OffLimitsStatus": "Off"
        },
        {
            "CompanyReferenceNumber": "C000064",
            "FileAs": "Deutsche Bank",
            "ItemType": "Companies",
            "TotalNumberOfAssignments": 1,
            "TypeClientChecked": true,
            "ItemId": "5d2c28b5-032b-4e17-ac9c-a8bfa2d94dff",
            "OffLimitsStatus": "Off"
        }
    ],
    "Paging": {
        "TotalItemCount": 2,
        "TotalDatabaseItemCount": 119
    }
}
```

This endpoint will return a list of `Company` type entities in the tenant database.

<aside class="notice">
    Please note, it's possible to POST a request body in a list endpoint request allowing you to define filters, sorting, grouping, column selection, pagination, and more.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/list`


## POST /api/v1/companies
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/companies' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "OptionalId": "009aadae-218e-4b4f-bc84-fd8551646cc1",
  "CompanyName": "Old Mutual",
  "ClientReference": "AS-192255",
  "RegistrationNumber": "091067503",
  "VATNumber": "GB119973511",
  "IsSupplier": true,
  "IsPartner": false,
  "IsClient": true,
  "IsPlaceOfStudy": false,
  "IsDoNotMailshotChecked": false,
  "IsDoNotContactChecked": false,
  "InternalComments": "Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Donec sed elit felis. Cras eu lacus id nunc interdum ultricies. In blandit sit amet tortor at condimentum.",
  "PaymentTerms": "21 Days",
  "ClientStatus": {
    "Id": "c084b8f7-7ccf-4aac-be0c-136e5acc6c28",
    "ItemDisplayText": "Current",
    "ItemType": "LookupListEntries"
  },
    "EmailAddresses": [
        {
            "IsPersonal": false,
            "IsBusiness": false,
            "IsVisibleAsDefault": true,
            "FieldName": "Email2Address",
            "DisplayTitle": "Email 2",
            "ItemValue": "accounts@lloydsbank.com"
        },
        {
            "IsPersonal": false,
            "IsBusiness": false,
            "IsVisibleAsDefault": true,
            "FieldName": "Email3Address",
            "DisplayTitle": "Email 3",
            "ItemValue": "finace@lloydsbank.com"
        },
        {
            "IsPersonal": false,
            "IsBusiness": false,
            "PreferredDisplayOrderIndexLegacy": 0,
            "FieldName": "Skype",
            "DisplayTitle": "Skype",
            "ItemValue": "finance@lloydsbank.com"
        }
    ],
    "PhoneNumbers": [
        {
            "IsPrimary": false,
            "PreferredDisplayOrderIndexLegacy": 1,
            "FormattedValue": "+44 (20) 7626 1880",
            "PhoneNumberComponents": {
                "Area": "20",
                "CountryCode": "44",
                "Local": "7626 1880",
                "Country": "United Kingdom",
                "PhoneCode": "44"
            },
            "IsVisibleAsDefault": false,
            "FieldName": "BusinessPhone2",
            "DisplayTitle": "Business 2 Tel",
            "ItemValue": "+44207626 1880"
        },
        {
            "IsPrimary": false,
            "FormattedValue": "+44 (20) 7626 7781",
            "PhoneNumberComponents": {
                "Area": "20",
                "CountryCode": "44",
                "Local": "7626 7781",
                "Country": "United Kingdom",
                "PhoneCode": "44"
            },
            "IsVisibleAsDefault": false,
            "FieldName": "AccountsPhone",
            "DisplayTitle": "Accounts Tel",
            "ItemValue": "+44207626 7781"
        },
        {
            "IsPrimary": false,
            "PreferredDisplayOrderIndex": 2,
            "FormattedValue": "+44 (20) 7626 7789",
            "PhoneNumberComponents": {
                "Area": "20",
                "CountryCode": "44",
                "Local": "7626 7789",
                "Country": "United Kingdom",
                "PhoneCode": "44"
            },
            "IsVisibleAsDefault": false,
            "FieldName": "SalesPhone",
            "DisplayTitle": "Sales Tel",
            "ItemValue": "+44207626 7789"
        },
        {
            "IsPrimary": false,
            "PreferredDisplayOrderIndex": 1,
            "FormattedValue": "+44 (20) 7626 1791",
            "PhoneNumberComponents": {
                "Area": "20",
                "CountryCode": "44",
                "Local": "7626 1791",
                "Country": "United Kingdom",
                "PhoneCode": "44"
            },
            "IsVisibleAsDefault": false,
            "FieldName": "SupportPhone",
            "DisplayTitle": "Support Tel",
            "ItemValue": "+44207626 1791"
        }
    ],
  "Websites": [
    {
      "IsCustomWebsite": false,
      "Id": "0b10da2c-7281-4f9a-8592-94d5e6a16bfe",
      "IsVisibleAsDefault": true,
      "FieldName": "WebPage",
      "ItemValue": "https://www.oldmutual.com/"
    }
  ],
  "Synonyms": "BH",
  "Revenue": 219900000,
  "RevenueCurrency": "30bb0085-c7fc-414a-bfc0-f56c66f62ae5",
  "NumberOfEmployees": "a8717b80-aa85-49bf-ade3-6b07254e0e5b",
  "CompanyType": 1,
  "BillingCurrency": "30bb0085-c7fc-414a-bfc0-f56c66f62ae5",
  "IsTopLevelCompany": true
}'
```

> Please note, a successful request will return a 201 created code.

This endpoint will allow you to create a new `Company` entity in the tenant database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies`

<i>Table 1. Core Fields</i>

Field | Description
--------- | -----------
OptionalId | Specify the unique identifier for the `Company` entity.
CompanyName [required] | Specify the name of the `Company` entity.
ClientReference | Specify a custom reference number for the `Company` entity.
RegistrationNumber | Specify the registration number for the `Company` entity.
VATNumber | Specify the VAT number for the `Company` entity.
IsSupplier | Used to indicate if the `Company` entities relationship with your company is that of a 'Supplier'.
IsPartner | Used to indicate if the `Company` entities relationship with your company is that of a 'Partner'.
IsClient | Used to indicate if the `Company` entities relationship with your company is that of a 'Client'.
IsPlaceOfStudy | Used to indicate if the `Company` entitiy is a 'Place of Study'.
IsDoNotMailshotChecked | Used to define the mailing preference for the `Company` entity.
IsDoNotContactChecked | Used to define the contact preference for the `Company` entity.
InternalComments | Used to record internal commentary related to the `Company` entity.
PaymentTerms | Used to record the payment terms agreed with the `Company` entity.
ClientStatus | Used to indicate the client relationship status between the `Company` entity and your company (e.g. Current, Previous, etc).
Synonyms | Other names that the `Company` entity is commonly referred to by.
Revenue | The lastest revenue figure published by the company.
Revenuecurrency | The currency code for the currency the latest revenue figures have been published in.
NumberOfEmployees | The number of employees the company has.
CompanyType | The company type: <ul><li>Public (0)</li><li>Private (1)</li></ul>
BillingCurrency | The preferred currency code the company wishes to be billed in.
IsTopLevelCompany | Used to indicate if the customer considers a `Company` entity as one of their top customer `OR` if the company is a parent company (Based upon the customer preferred usage of this field).
ExternalId1 | One of three fields available to record a unique reference or identifier for the `Company` entity to reconcile it with other applications via API integrations. 
ExternalId2 | One of three fields available to record a unique reference or identifier for the `Company` entity to reconcile it with other applications via API integrations.
ExternalId3 | One of three fields available to record a unique reference or identifier for the `Company` entity to reconcile it with other applications via API integrations.

<i>Table 2. Email Address Array</i>

Field | Description
--------- | -----------
IsPersonal | Used to indicate if the email address is personal in nature.
IsBusiness | Used to indicate if the email address is used for business purposes. 
IsVisibleAsDefault | Used to deterime if the email address should be visible by default when openeing the `Company` entity record.
FieldName | The system name of the email fields for `Company` entities: <ul><li>Email2Address</li><li>Email3Address</li><li>Skype</li></ul>
DisplayTitle | The name of the email address field as it appears in Invenias applications in `Company` entities: <ul><li>Email 2</li><li>Email 3</li><li>Skype</li></ul>
ItemValue | Email Address (e.g. inveniassupport@bullhorn.com)

<i>Table 3. Telephone Number Array</i>

Field | Description
--------- | -----------
IsPrimary | Used to indicate if the number is primary number to be used internally to contact the `Company` entity.
PreferredDisplayOrderIndex | Defines the ordinal position of the contact field within the list of contact fields in the `Company` entity record when opened in Invenias applications.
FormattedValue | <ul><li>If autoformatting is disabled in Invenias system preferences please add the entire number here. You do not need to declare the 'PhoneNumberComponants' array in your request model.</li><li>If autoformatting is enabled, please exclude this field from your request model and add break down the number into the individual 'PhoneNumberComponents'.</li></ul>
Area | Area Code
Local | Local Code
Country | Country where number is registered.
PhoneCode | Country Code
IsVisibleAsDefault | There are many contact fields in Invenias. Many are hidden by default and can only be viewed by expanding a pick-list. This field can be used to declare if this should be immediately visible in the 'Contact' tab in the person record. <br></br>Accepted Values: <ul> <li>true</li> <li>false</li></ul>
FieldName | Accepted Values: <ul><li>BusinessPhone2</li><li>AccountsPhone</li><li>SalesPhone</li><li>SupportPhone</li><li>ISDN</li>

Please note, there is a setting in the 'System Preferences' that will impact the way you add telephone numbers when creating a person record. Please check
this before you proceed.

Behaviours:
<ul>
<li>If you pass in the phone number components and you have autoformatting enabled, we try to format the telephone according to the
components provided.</li>
<li>If you pass in the phone number components and the autoformatting is disabled, we take whatever is in the 'FormattedValue' and
persist it.</li>
<li>If you do not pass in phone number components, we try to persist whatever is stored in the ItemValue field (depending on autoformatting
setting value it will or will not get formatted before saving).</li>
</ul>

## GET /api/v1/companies/{id}
> Example (cURL)
```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/companies/009aadae-218e-4b4f-bc84-fd8551646cc1' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
{
    "Id": "009aadae-218e-4b4f-bc84-fd8551646cc1",
    "CompanyNumber": "C000124 (CSEV-136464)",
    "IsInveniasCompany": false,
    "PublishedAssignmentsCount": 0,
    "IsFavourite": false,
    "EntityDetails": {
        "DateCreated": "2021-12-20T14:50:35.9618806+00:00",
        "DateModified": "2021-12-20T14:50:35.9618806+00:00",
        "CreatedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "b91c87bd-0f45-4441-92bd-2c19f79c8af5",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "CompanyName": "Old Mutual",
    "ClientReference": "AS-192255",
    "RegistrationNumber": "091067503",
    "VATNumber": "GB119973511",
    "IsSupplier": true,
    "IsPartner": false,
    "IsClient": true,
    "IsPlaceOfStudy": false,
    "IsDoNotMailshotChecked": false,
    "IsDoNotContactChecked": false,
    "InternalComments": "Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Donec sed elit felis. Cras eu lacus id nunc interdum ultricies. In blandit sit amet tortor at condimentum.",
    "PaymentTerms": "21 Days",
    "ClientStatus": {
        "Id": "c084b8f7-7ccf-4aac-be0c-136e5acc6c28",
        "DisplayTitle": "Client Status",
        "ItemDisplayText": "Current",
        "ItemType": "LookupListEntries"
    },
    "EmailAddresses": [
        {
            "IsPersonal": false,
            "IsBusiness": false,
            "IsVisibleAsDefault": true,
            "FieldName": "Email2Address",
            "DisplayTitle": "Email 2",
            "ItemValue": "accounts@lloydsbank.com"
        }
    ],
    "PhoneNumbers": [
        {
            "IsPrimary": false,
            "FormattedValue": "+44 (20) 7626 1880",
            "PhoneNumberComponents": {
                "Area": "20",
                "CountryCode": "44",
                "Local": "7626 1880",
                "Country": "United Kingdom",
                "PhoneCode": "44"
            },
            "IsVisibleAsDefault": false,
            "FieldName": "BusinessPhone2",
            "DisplayTitle": "Business 2 Tel",
            "ItemValue": "+44207626 1880"
        }
    ],
    "MarketCapCurrency": "3a0e9471-35e2-462f-86dd-f4b08dc70708",
    "Revenue": 219900000.0,
    "RevenueCurrency": "3a0e9471-35e2-462f-86dd-f4b08dc70708",
    "NumberOfEmployees": "a8717b80-aa85-49bf-ade3-6b07254e0e5b",
    "CompanyType": 1,
    "BillingCurrency": "3a0e9471-35e2-462f-86dd-f4b08dc70708",
    "IsTopLevelCompany": true...
}
```

This endpoint will return information about any given `Company` entity in the tenant database.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.


## PUT /api/v2/companies/{id}
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/companies/{id}' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "DefaultLocation": {
    "LocationName": "Reading, United Kingdom (Head Office)",
    "AddressComponents": {
      "FullAddress": "Atlas Executive Search\nForbury Square\nReading\nBerkshire\nRG1 3EU\nUNITED KINGDOM",
      "Street": "Atlas Search\nForbury Square",
      "TownCity": "Reading",
      "County": "Berkshire",
      "Postcode": "RG1 3EU",
      "Country": "UNITED KINGDOM"
    },
    "Default": true
  },
  "CompanyName": "Atlas Executive Search",
  "Synonyms": "AES, Atlas Search"
}
```

> Example Response (JSON)
```shell
{
    "Id": "15b22477-90dc-4d41-8a39-e88cee7e1a79",
    "CompanyNumber": "C000001 (ATLAS)",
    "DefaultLocation": {
        "LocationId": "7ddaf324-2426-44e2-8420-feb7d1c3debb",
        "EntityDetails": {
            "DateCreated": "2022-01-19T12:41:07.6593911+00:00",
            "DateModified": "2022-01-31T15:02:09.111112+00:00",
            "CreatedBy": {
                "Id": "e459823e-61a4-45a5-b7c9-5dee1809d88f",
                "ItemType": "Users"
            },
            "ModifiedBy": {
                "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
                "ItemDisplayText": "Glen Chamberlain",
                "ItemType": "Users"
            },
            "Owner": {
                "Id": "e459823e-61a4-45a5-b7c9-5dee1809d88f",
                "ItemType": "Users"
            }
        },
        "LocationName": "Reading, United Kingdom (Head Office)",
        "AddressComponents": {
            "FullAddress": "Atlas Search\nForbury Square\r\nReading\r\nBerkshire\r\nRG1 3EU\r\nUNITED KINGDOM",
            "Street": "Atlas Search\nForbury Square",
            "TownCity": "Reading",
            "County": "Berkshire",
            "Postcode": "RG1 3EU",
            "Country": "UNITED KINGDOM"
        },
        "Default": true
    },
    "CompanyName": "Atlas Executive Search",
    "Synonyms": "AES, Atlas Search"
}...
```
This endpoint allows you to replace a representation of the target `Company` entity with the request payload.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.

## DELETE /api/v1/companies/{id}
> Example (cURL)
```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/companies/{id}' \
--header 'Authorization: Bearer {token}'
```

> Please note, successful requests will return a 200 OK response code.

The DELETE /api/v1/companies/{id} endpoint is used to `permanently` delete a single Company entity per request.

<aside class="notice">
    Please note, when deleting a 'Company' entity, it will also delete any relations that exist between it and other core entities.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.

## PUT /api/v1/companies/bulkdelete
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/companies/bulkdelete' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "ItemReferences": [
        {
            "Id": "9bc03445-fec4-43e2-a5f1-00030c8f40de"
        },
        {
            "Id": "bd97bbf5-e88a-454d-a7fb-0017eb665282"
        },
        {
            "Id": "c261e3d2-ced5-4430-9a85-0023aa3284a2"
        }
    ]
}'
```

> Please note, successful requests will return a 200 OK response code.

The PUT /api/v1/companies/bulkdelete endpoint is used to `permanently` delete more than one `Company` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/bulkdelete`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
ids | [required] | Specify the unique identifiers for the `Company` entities you wish to delete.

## POST /api/v1/companies/{id}/students/list
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/companies/{id}/students/list' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{}'
```

> Example Response (JSON)
```shell
{
    "Items": [
        {
            "CompanyDisplayName": "Atlas Executive Search",
            "EndDate": "2006-09-01T00:00:00+01:00",
            "ItemType": "PersonEducation",
            "PersonDisplayName": "Glen Chamberlain",
            "PersonId": {
                "Id": "b6a53fe4-8fbd-4393-b718-af57a92e896c"
            },
            "PlaceOfStudy": "The Academy of Contemporary Music",
            "PositionJobTitle": "Chief Executive Officer",
            "Qualification": {
                "Id": "a387d3f6-100f-4aa7-aa60-ad0257d516b9",
                "ItemDisplayText": "HND, HNC or Equivalent"
            },
            "StartDate": "2004-09-01T00:00:00+01:00",
            "Subject": "Music Prodicution",
            "ItemId": "e815e30d-08d3-43df-82a6-84b5755a0344",
            "OffLimitsStatus": "Off"
        }
    ]
}
```

Returns a list `People` entities who have one or more `Education` entities relationally linked to a `Company` entity including their related qualifications.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/students/list`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.


## POST /api/v1/companies/{id}/people/list
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/companies/{id}/people/list' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "IncludeAdditionalValues": false,
    "UseLookUpViewDefinition": false,
    "IncludeDisplayViews": false,
    "IncludeAvailableColumns": false,
    "IncludeCategories": false
}'
```

> Example Response (JSON)
```shell
{
    "Items": [
        {
            "CompanyID": {
                "Id": "655d0786-d1c9-4b7e-ac82-02b3bab153ab"
            },
            "ItemType": "Positions",
            "JobTitle": "Assest Manager",
            "PersonDisplayName": "Austin Hayward",
            "PersonId": {
                "Id": "7f528181-390b-4bcf-9360-441afd037d66"
            },
            "ItemId": "3488c224-7f83-439e-b3c1-70b97ed2c6a1",
            "OffLimitsStatus": "Off"
        },
        {
            "CompanyID": {
                "Id": "655d0786-d1c9-4b7e-ac82-02b3bab153ab"
            },
            "ItemType": "Positions",
            "JobTitle": "Facilities Planning Specialist",
            "PersonDisplayName": "Douglas Mitchell",
            "PersonId": {
                "Id": "b984777b-a98b-45a1-8915-9fb69f89e7db"
            },
            "ItemId": "daab9cf5-f4f9-4f2f-8e65-2e4120f46da1",
            "OffLimitsStatus": "Off"
        }
    ]
}
```
Returns a list `People` entities who have one or more `Position` entities relationally linked to a `Company`.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/people/list`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.

## POST /api/v1/companies/{id}/bulkremovepeople
> Example (cURL)
```shell
curl --location -g --request POST 'https://{subdomain}.invenias.com/api/v1/companies/{id}/bulkremovepeople' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "ItemReferences": [
    {
      "Id": "7f528181-390b-4bcf-9360-441afd037d66"
    },
        {
      "Id": "b984777b-a98b-45a1-8915-9fb69f89e7db"
    },
        {
      "Id": "2d492917-07c1-4df8-97cb-bdac59ba5595"
    }
  ]
}'
```

> Please note, successful requests will return a 200 OK response code.

Deletes the relationship between one or more `People` entities that are relationally linked via one or more `Position` entities to any given `Company` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/bulkremovepeople`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.

## POST /api/v1/companies/{id}/currentpeople/list
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/companies/{id}/currentpeople/list' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "IncludeAdditionalValues": false,
    "UseLookUpViewDefinition": false,
    "IncludeDisplayViews": false,
    "IncludeAvailableColumns": false,
    "IncludeCategories": false
}'
```

> Example Response (JSON)
```shell
{
    "Items": [
        {
            "CompanyDisplayName": "VonRueden, Turcotte and Smith",
            "DisplayFileAs": "Douglas Mitchell",
            "FileAs": "Mr Douglas Mitchell",
            "ItemType": "People",
            "PositionJobTitle": "Facilities Planning Specialist",
            "ItemId": "b984777b-a98b-45a1-8915-9fb69f89e7db",
            "OffLimitsStatus": "Off"
        },
        {
            "CompanyDisplayName": "VonRueden, Turcotte and Smith",
            "DisplayFileAs": "Harley Ball",
            "FileAs": "Mr Harley Ball",
            "ItemType": "People",
            "PositionJobTitle": "C++ Software Engineer",
            "ItemId": "2d492917-07c1-4df8-97cb-bdac59ba5595",
            "OffLimitsStatus": "Off"
        },
        {
            "CompanyDisplayName": "VonRueden, Turcotte and Smith",
            "DisplayFileAs": "Harrison Grant",
            "FileAs": "Mr Harrison Grant",
            "ItemType": "People",
            "PositionJobTitle": "O2 Piping Inspector",
            "ItemId": "b3838025-c785-435e-a730-b7d5b77286a1",
            "OffLimitsStatus": "Off"
        }
}
```
Returns a list of `People` entities who have one or more (Current) `Position` entities relationally linked to a `Company`.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/currentpeople/list`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity

## POST /api/v1/companies/{id}/clientassignments/list
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/companies/{id}/clientassignments/list' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "IncludeAdditionalValues": false,
    "UseLookUpViewDefinition": false,
    "IncludeDisplayViews": false,
    "IncludeAvailableColumns": false,
    "IncludeCategories": false
}'
```

> Example Response (JSON)
```shell
{
    "Items": [
        {
            "FileAs": "Facilities Planning Specialist",
            "ItemType": "Assignments",
            "Status": {
                "Id": "a9d5ebc1-8414-402f-aab0-79e175d5ec55",
                "ItemDisplayText": "Forecasted"
            },
            "ItemId": "3f54ef7b-80b8-40f0-9ac0-231c208f3d13",
            "OffLimitsStatus": "Off"
        },
        {
            "FileAs": "Pneumatic Technician",
            "ItemType": "Assignments",
            "Status": {
                "Id": "12a6b6f2-f121-47d4-870c-2c93fee84a71",
                "ItemDisplayText": "Active"
            },
            "ItemId": "75dcf8ac-bb99-4936-b002-54e78aff8b44",
            "OffLimitsStatus": "Off"
        }
    ]
}
```
Returns a list of `Assignment` entities relationally linked to any given `Company`.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/clientassignments/list`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity

## POST /api/v1/companies/{id}/programmes/list
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/companies/{id}/programmes/list' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "IncludeAdditionalValues": false,
    "UseLookUpViewDefinition": false,
    "IncludeDisplayViews": false,
    "IncludeAvailableColumns": false,
    "IncludeCategories": false
}'
```

> Example Response (JSON)
```shell
{
    "Items": [
        {
            "FileAs": "C-Suite Talent Pool / 2710 Aerospace & Defense",
            "ItemType": "Programmes",
            "Name": "C-Suite Talent Pool / 2710 Aerospace & Defense",
            "ProgrammeStatusDisplay": "Active",
            "ProgrammeType": "Talent Pool",
            "ProgrammeTypeId": {
                "Id": "ee64fef1-182c-40fc-9fd0-c31c0198aab6"
            },
            "Relation_ownerid": {
                "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
                "ItemDisplayText": "Glen Chamberlain"
            },
            "Relation_progressstatus": {
                "Id": "268e3751-9c1e-405a-8730-7300e524f56f",
                "ItemDisplayText": "01. Identified"
            },
            "ItemId": "1839c80f-d80a-41e6-806c-f696a82cb015",
            "OffLimitsStatus": "Off"
        }
    ]
}
```
Returns a list of `Programme` entities relationally linked to any given `Company`.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/programmes/list`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.

## GET /api/v1/companies/{id}/programmes/{programmeId}
> Example (cURL)
```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/companies/{id}/programmes/{programmeId}' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
{
    "Programme": {
        "Id": "1839c80f-d80a-41e6-806c-f696a82cb015",
        "ProgrammeName": "C-Suite Talent Pool /  2710 Aerospace & Defense",
        "ProgrammeType": {
            "ProgrammeStatuses": {
                "ItemReferences": [
                    {
                        "Id": "2881693a-7608-4403-8d65-47c3234f26d6",
                        "DisplayTitle": "Active",
                        "ItemDisplayText": "Active",
                        "ItemType": "LookupListEntries"
                    },
                    {
                        "Id": "2ae7e58c-c854-4bb3-9499-07b125d42763",
                        "DisplayTitle": "Completed",
                        "ItemDisplayText": "Completed",
                        "ItemType": "LookupListEntries"
                    },
                    {
                        "Id": "da69a21d-33a8-423c-8311-41b74dd97514",
                        "DisplayTitle": "Cancelled",
                        "ItemDisplayText": "Cancelled",
                        "ItemType": "LookupListEntries"
                    }
                ]
            },
            "PeopleProgressStatuses": {
                "ItemReferences": [
                    {
                        "Id": "4b31c1c5-11c5-4b3a-8eb5-ecf4d31cab5e",
                        "DisplayTitle": "01. Identified",
                        "ItemDisplayText": "01. Identified",
                        "ItemType": "LookupListEntries"
                    },
                    {
                        "Id": "6efee606-f880-4cde-b80c-19335ab405ca",
                        "DisplayTitle": "02. In Contact",
                        "ItemDisplayText": "02. In Contact",
                        "ItemType": "LookupListEntries"
                    },
                    {
                        "Id": "8e8609ba-6919-4c7e-bc68-5878915d754c",
                        "DisplayTitle": "03. Following",
                        "ItemDisplayText": "03. Following",
                        "ItemType": "LookupListEntries"
                    },
                    {
                        "Id": "163c6cf6-bde2-4207-8ee7-b76fccabf669",
                        "DisplayTitle": "X. Not Relevant",
                        "ItemDisplayText": "X. Not Relevant",
                        "ItemType": "LookupListEntries"
                    }
                ]
            },
            "CompaniesProgressStatuses": {
                "ItemReferences": [
                    {
                        "Id": "268e3751-9c1e-405a-8730-7300e524f56f",
                        "DisplayTitle": "01. Identified",
                        "ItemDisplayText": "01. Identified",
                        "ItemType": "LookupListEntries"
                    },
                    {
                        "Id": "6136b5bc-3698-43b8-939f-2a4d63abe6b1",
                        "DisplayTitle": "02. Initial Research",
                        "ItemDisplayText": "02. Initial Research",
                        "ItemType": "LookupListEntries"
                    },
                    {
                        "Id": "e3b2ed82-f043-4ecc-9d9a-3f6092f742d2",
                        "DisplayTitle": "03. Ongoing Research",
                        "ItemDisplayText": "03. Ongoing Research",
                        "ItemType": "LookupListEntries"
                    },
                    {
                        "Id": "c993126a-abdf-4d94-9102-dad8317455bd",
                        "DisplayTitle": "X. Not Relevant",
                        "ItemDisplayText": "X. Not Relevant",
                        "ItemType": "LookupListEntries"
                    },
                    {
                        "Id": "e9bd4c34-fb56-4da8-90f5-ca4894cce6a4",
                        "DisplayTitle": "X. Company Acquired",
                        "ItemDisplayText": "X. Company Acquired",
                        "ItemType": "LookupListEntries"
                    }
                ]
            },
            "Id": "ee64fef1-182c-40fc-9fd0-c31c0198aab6",
            "ProgrammeTypeName": "Talent Pool",
            "IsEnabled": true,
            "IsCategoriesEnabled": true,
            "IsInternalCommentsEnabled": true,
            "IsPeopleListEnabled": true,
            "IsCompaniesListEnabled": true,
            "IsMilestonesListEnabled": false,
            "IsBillingEventsListEnabled": false
        }
    },
    "EntityDetails": {
        "DateCreated": "2022-02-17T13:48:17.4559206+00:00",
        "DateModified": "2022-02-17T13:48:17.4559206+00:00",
        "CreatedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "ModifiedBy": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        },
        "Owner": {
            "Id": "f5d91fe7-cfb3-4594-a3a6-dfcd529d4fb3",
            "ItemDisplayText": "Glen Chamberlain",
            "ItemType": "Users"
        }
    },
    "RecordStatus": {
        "Id": "268e3751-9c1e-405a-8730-7300e524f56f",
        "DisplayTitle": "01. Identified",
        "ItemDisplayText": "01. Identified",
        "ItemType": "LookupListEntries"
    }
}
```
Returns details related to any given `Programme` entity which is relationally linked to any a `Company` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/programmes/{programmeId}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.
programmeId | [required] | Specify the unique identifier for the releated `Programme` entity.

## DELETE /api/v1/companies/{id}/programmes/{programmeId}
> Example (cURL)
```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/companies/{id}/programmes/{programmeId}' \
--header 'Authorization: Bearer {token}'
```

> Please note, successful requests will return a 200 OK response code.

Removes the relationship between a `Company` entity and a `Programme` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/programmes/{programmeId}`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.
programmeId | [required] | Specify the unique identifier for the releated `Programme` entity.

## POST /api/v1/companies/{id}/companies
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/companies/{id}/companies' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "RelationCompany": {
        "Id": "d9befec3-8a95-4057-8144-00017cb354fe"
    },
    "Relationship": "Subsidiary Company"
}'
```

> Please note, successful requests will return a 204 No Content response code.

Creates a relation between two `Company` entities (e.g. Competitor, Parent, Subsidiary, Investor, Supplier, etc).

Accepted Company Relationship Values:
<ul>
    <li>Partner</li>
    <li>Parent Company</li>
    <li>Subsidiary Company</li>
    <li>Investor</li>
    <li>Portfolio/Investment</li>
    <li>Client</li>
    <li>Supplier</li>
    <li>Competitor</li>
</ul>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/companies`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.


## PUT /api/v1/companies/{id}/companies
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/companies/{id}/companies' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "RelationCompany": {
        "Id": "d9befec3-8a95-4057-8144-00017cb354fe"
    },
    "Relationship": "Parent Company"
}'
```

> Please note, successful requests will return a 204 No Content response code.

Allows you to update the relationship between any given `Company` entity and another.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/companies`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.

## POST /api/v1/companies/{id}/relations/list
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/companies/{id}/relations/list' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "IncludeAdditionalValues": false,
    "UseLookUpViewDefinition": false,
    "IncludeDisplayViews": false,
    "IncludeAvailableColumns": false,
    "IncludeCategories": false
}'
```

> Example Response (JSON)
```shell
{
    "Items": [
        {
            "FileAs": "Díaz de Villalobos",
            "ItemType": "Companies",
            "RelationId": {
                "Id": "9d2f523c-31aa-48da-825c-370468e827ad"
            },
            "RelationName": "Partner",
            "ItemId": "d9befec3-8a95-4057-8144-00017cb354fe",
            "OffLimitsStatus": "Off"
        }
    ]
}
```
Returns a lists of `Company` entites relationally linked to any given `Company` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/relations/list`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.

## POST /api/v1/companies/{id}/bulkremovecompanies
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/companies/{id}/bulkremovecompanies' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "Relations": [
    {
      "RelationId": "d9befec3-8a95-4057-8144-00017cb354fe",
      "Relationship": "Partner"
    }
  ]
}'
```

> Please note, successful requests will return a 204 No Content response code.

Used to permanently delete one or more `Company` entity relationships linked to any given `Company` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/bulkremovecompanies`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.

## POST /api/v1/companies/{id}/externalid1
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/companies/{id}/externalId1?externalId1=e354ef4d-052d-4e90-8c8d-52c31e780061' \
--header 'Authorization: Bearer {token}'
```

> Please note, successful requests will return a 200 OK response code.

Allows you to add an unique external identifier for the Invenias `Company` entity to be leveraged by an external application where applicable.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/externalid1`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.
externalId1 | [required] | Specify the external identifier for the desired `Company` entity.

## DELETE /api/v1/companies/{id}/externalid1
> Example (cURL)
```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/companies/{id}/externalId1' \
--header 'Authorization: Bearer {token}'
```

> Please note, successful requests will return a 204 No Content code.

Allows you to delete an unique external identifier for the Invenias `Company` entity

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/externalid1`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.

## GET /api/v1/companies/{id}/notepad
> Example (cURL)
```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/companies/{id}/notepad' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
{
    "Id": "a5037d6a-795d-4b86-83ce-b16cb11e6c14",
    "Text": "Ipsum Lorem\r\n\r\nLorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean lobortis nunc eget nisi euismod, ac tristique dolor luctus. Praesent consequat euismod tortor semper volutpat. Praesent in dui a lectus bibendum elementum. Etiam facilisis interdum erat, ut pretium leo condimentum eget. Integer maximus gravida nibh in posuere. Sed rhoncus pharetra est, ac consectetur nisl ultrices sed. Quisque in porttitor ligula. Nulla facilisi.  Integer at feugiat tortor.",
    "Html": "\r\n<h1>Ipsum Lorem</h1><p>\r\nLorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean lobortis nunc eget nisi euismod, ac tristique dolor luctus. Praesent consequat euismod tortor semper volutpat. Praesent in dui a lectus bibendum elementum. Etiam facilisis interdum erat, ut pretium leo condimentum eget. Integer maximus gravida nibh in posuere. Sed rhoncus pharetra est, ac consectetur nisl ultrices sed. Quisque in porttitor ligula. Nulla facilisi. Morbi rhoncus sem id accumsan finibus. <br></br>Vivamus pharetra ex id dictum tristique. Sed scelerisque finibus tempor. Maecenas eu luctus neque, eget congue leo. Vivamus sollicitudin malesuada scelerisque. Curabitur erat massa, tincidunt ut congue vitae, tempus sit amet orci. Vivamus varius dolor lobortis, rhoncus dui a, viverra eros. Ut tellus ante, gravida quis lectus et, malesuada auctor metus. Aenean a tincidunt mauris.</p><ul>\r\n<li>Phasellus varius <strong>ligula</strong> sit amet ante elementum.</li>\r\n<li>Ut tellus ante, gravida quis lectus et, malesuada auctor metus. Aenean a tincidunt mauris.</li>\r\n<li>Morbi pharetra posuere orci. Nulla facilisi. Nulla quis nunc nisl.</li>\r\n</ul>\r\n\r\n<p><i>Maecenas molestie tristique lacus, a mattis mi tempor a. Nunc ex nisi, aliquet sed bibendum vel, varius sit amet tortor. Integer at feugiat tortor.</i></p>"
}
```

Used to retrieve the plain text or HTML content of the 'Notepad' field for any given `Company` entity.

<aside class="warning">
Please note, if the content of the 'Notepad' is null, you will receive the following client error code 404 Not Found, accompanied by the message "Item not found". If you receive this error, it means the 'Notepad' field is null or the 'Company' entity does not exist within the database.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/notepad`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.

## PUT /api/v1/companies/{id}/notepad
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/{id}/notepad' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Html": "
<h1>Ipsum Lorem</h1><p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean lobortis nunc eget nisi euismod, ac tristique dolor luctus. Praesent consequat euismod tortor semper volutpat. Praesent in dui a lectus bibendum elementum. Etiam facilisis interdum erat, ut pretium leo condimentum eget. Integer maximus gravida nibh in posuere. Sed rhoncus pharetra est, ac consectetur nisl ultrices sed. Quisque in porttitor ligula. Nulla facilisi. Morbi rhoncus sem id accumsan finibus. <br></br>Vivamus pharetra ex id dictum tristique. Sed scelerisque finibus tempor. Maecenas eu luctus neque, eget congue leo. Vivamus sollicitudin malesuada scelerisque. Curabitur erat massa, tincidunt ut congue vitae, tempus sit amet orci. Vivamus varius dolor lobortis, rhoncus dui a, viverra eros. Ut tellus ante, gravida quis lectus et, malesuada auctor metus. Aenean a tincidunt mauris.</p><ul>
<li>Phasellus varius <strong>ligula</strong> sit amet ante elementum.</li>
<li>Ut tellus ante, gravida quis lectus et, malesuada auctor metus. Aenean a tincidunt mauris.</li>
<li>Morbi pharetra posuere orci. Nulla facilisi. Nulla quis nunc nisl.</li>
</ul><p><i>Maecenas molestie tristique lacus, a mattis mi tempor a. Nunc ex nisi, aliquet sed bibendum vel, varius sit amet tortor. Integer at feugiat tortor.</i></p>"
}'
```

> Example Response (JSON)
```shell
{
    "Id": "a5037d6a-795d-4b86-83ce-b16cb11e6c14",
    "Text": "Ipsum Lorem\r\n\r\nLorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean lobortis nunc eget nisi euismod, ac tristique dolor luctus. Praesent consequat euismod tortor semper volutpat. Praesent in dui a lectus bibendum elementum. Etiam facilisis interdum erat, ut pretium leo condimentum eget. Integer maximus gravida nibh in posuere. Sed rhoncus pharetra est, ac consectetur nisl ultrices sed. Quisque in porttitor ligula. Nulla facilisi. Morbi rhoncus sem id accumsan finibus. Vivamus pharetra ex id dictum tristique. Sed scelerisque finibus tempor. Maecenas eu luctus neque, eget congue leo. Vivamus sollicitudin malesuada scelerisque. Curabitur erat massa, tincidunt ut congue vitae, tempus sit amet orci. Vivamus varius dolor lobortis, rhoncus dui a, viverra eros. Ut tellus ante, gravida quis lectus et, malesuada auctor metus. Aenean a tincidunt mauris.Phasellus varius ligula sit amet ante elementum.Ut tellus ante, gravida quis lectus et, malesuada auctor metus. Aenean a tincidunt mauris.Morbi pharetra posuere orci. Nulla facilisi. Nulla quis nunc nisl.\r\nMaecenas molestie tristique lacus, a mattis mi tempor a. Nunc ex nisi, aliquet sed bibendum vel, varius sit amet tortor. Integer at feugiat tortor.",
    "Html": "\r\n<h1>Ipsum Lorem</h1><p>\r\nLorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean lobortis nunc eget nisi euismod, ac tristique dolor luctus. Praesent consequat euismod tortor semper volutpat. Praesent in dui a lectus bibendum elementum. Etiam facilisis interdum erat, ut pretium leo condimentum eget. Integer maximus gravida nibh in posuere. Sed rhoncus pharetra est, ac consectetur nisl ultrices sed. Quisque in porttitor ligula. Nulla facilisi. Morbi rhoncus sem id accumsan finibus. <br></br>Vivamus pharetra ex id dictum tristique. Sed scelerisque finibus tempor. Maecenas eu luctus neque, eget congue leo. Vivamus sollicitudin malesuada scelerisque. Curabitur erat massa, tincidunt ut congue vitae, tempus sit amet orci. Vivamus varius dolor lobortis, rhoncus dui a, viverra eros. Ut tellus ante, gravida quis lectus et, malesuada auctor metus. Aenean a tincidunt mauris.</p><ul>\r\n<li>Phasellus varius <strong>ligula</strong> sit amet ante elementum.</li>\r\n<li>Ut tellus ante, gravida quis lectus et, malesuada auctor metus. Aenean a tincidunt mauris.</li>\r\n<li>Morbi pharetra posuere orci. Nulla facilisi. Nulla quis nunc nisl.</li>\r\n</ul>\r\n\r\n<p><i>Maecenas molestie tristique lacus, a mattis mi tempor a. Nunc ex nisi, aliquet sed bibendum vel, varius sit amet tortor. Integer at feugiat tortor.</i></p>"
}
```

Allows you to populate 'Notepad' field for the given `Company` entity using either HTML or Plain Text.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/notepad`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.

## GET /api/v1/companies/{id}/profile
> Example (cURL)
```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/companies/{id}/profile?external=false' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
{
    "Id": "a5037d6a-795d-4b86-83ce-b16cb11e6c14",
    "Text": "Ipsum Lorem\r\n\r\nLorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean lobortis nunc eget nisi euismod, ac tristique dolor luctus. Praesent consequat euismod tortor semper volutpat. Praesent in dui a lectus bibendum elementum. Etiam facilisis interdum erat, ut pretium leo condimentum eget. Integer maximus gravida nibh in posuere. Sed rhoncus pharetra est, ac consectetur nisl ultrices sed. Quisque in porttitor ligula. Nulla facilisi.  Integer at feugiat tortor.",
    "Html": "\r\n<h1>Ipsum Lorem</h1><p>\r\nLorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean lobortis nunc eget nisi euismod, ac tristique dolor luctus. Praesent consequat euismod tortor semper volutpat. Praesent in dui a lectus bibendum elementum. Etiam facilisis interdum erat, ut pretium leo condimentum eget. Integer maximus gravida nibh in posuere. Sed rhoncus pharetra est, ac consectetur nisl ultrices sed. Quisque in porttitor ligula. Nulla facilisi. Morbi rhoncus sem id accumsan finibus. <br></br>Vivamus pharetra ex id dictum tristique. Sed scelerisque finibus tempor. Maecenas eu luctus neque, eget congue leo. Vivamus sollicitudin malesuada scelerisque. Curabitur erat massa, tincidunt ut congue vitae, tempus sit amet orci. Vivamus varius dolor lobortis, rhoncus dui a, viverra eros. Ut tellus ante, gravida quis lectus et, malesuada auctor metus. Aenean a tincidunt mauris.</p><ul>\r\n<li>Phasellus varius <strong>ligula</strong> sit amet ante elementum.</li>\r\n<li>Ut tellus ante, gravida quis lectus et, malesuada auctor metus. Aenean a tincidunt mauris.</li>\r\n<li>Morbi pharetra posuere orci. Nulla facilisi. Nulla quis nunc nisl.</li>\r\n</ul>\r\n\r\n<p><i>Maecenas molestie tristique lacus, a mattis mi tempor a. Nunc ex nisi, aliquet sed bibendum vel, varius sit amet tortor. Integer at feugiat tortor.</i></p>"
}
```

Used to retrieve the plain text or HTML content of the internal/external 'Overview' field for any given `Company` entity.

<aside class="warning">
Please note, if the content of the 'Overview' is null, you will receive the following client error code 404 Not Found, accompanied by the message "Item not found". If you receive this error, it means the 'Overview' field is null or the 'Company' entity does not exist within the database.
</aside>

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/profile`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.
external | [required] | Specify if you wish to get the internal or external 'Overview' for the desired `Company` entity.

## PUT /api/v1/companies/{id}/profile
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/companies/{id}/profile?external=false' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Html": "
<h1>Ipsum Lorem</h1><p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean lobortis nunc eget nisi euismod, ac tristique dolor luctus. Praesent consequat euismod tortor semper volutpat. Praesent in dui a lectus bibendum elementum. Etiam facilisis interdum erat, ut pretium leo condimentum eget. Integer maximus gravida nibh in posuere. Sed rhoncus pharetra est, ac consectetur nisl ultrices sed. Quisque in porttitor ligula. Nulla facilisi. Morbi rhoncus sem id accumsan finibus. <br></br>Vivamus pharetra ex id dictum tristique. Sed scelerisque finibus tempor. Maecenas eu luctus neque, eget congue leo. Vivamus sollicitudin malesuada scelerisque. Curabitur erat massa, tincidunt ut congue vitae, tempus sit amet orci. Vivamus varius dolor lobortis, rhoncus dui a, viverra eros. Ut tellus ante, gravida quis lectus et, malesuada auctor metus. Aenean a tincidunt mauris.</p><ul>
<li>Phasellus varius <strong>ligula</strong> sit amet ante elementum.</li>
<li>Ut tellus ante, gravida quis lectus et, malesuada auctor metus. Aenean a tincidunt mauris.</li>
<li>Morbi pharetra posuere orci. Nulla facilisi. Nulla quis nunc nisl.</li>
</ul>

<p><i>Maecenas molestie tristique lacus, a mattis mi tempor a. Nunc ex nisi, aliquet sed bibendum vel, varius sit amet tortor. Integer at feugiat tortor.</i></p>"
}'
```

> Example Response (JSON)
```shell
{
    "Id": "831e1be9-42f9-4479-8643-6b75014f1299",
    "Text": "Ipsum Lorem\r\n\r\nLorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean lobortis nunc eget nisi euismod, ac tristique dolor luctus. Praesent consequat euismod tortor semper volutpat. Praesent in dui a lectus bibendum elementum. Etiam facilisis interdum erat, ut pretium leo condimentum eget. Integer maximus gravida nibh in posuere. Sed rhoncus pharetra est, ac consectetur nisl ultrices sed. Quisque in porttitor ligula. Nulla facilisi. Morbi rhoncus sem id accumsan finibus. Vivamus pharetra ex id dictum tristique. Sed scelerisque finibus tempor. Maecenas eu luctus neque, eget congue leo. Vivamus sollicitudin malesuada scelerisque. Curabitur erat massa, tincidunt ut congue vitae, tempus sit amet orci. Vivamus varius dolor lobortis, rhoncus dui a, viverra eros. Ut tellus ante, gravida quis lectus et, malesuada auctor metus. Aenean a tincidunt mauris.Phasellus varius ligula sit amet ante elementum.Ut tellus ante, gravida quis lectus et, malesuada auctor metus. Aenean a tincidunt mauris.Morbi pharetra posuere orci. Nulla facilisi. Nulla quis nunc nisl.\r\nMaecenas molestie tristique lacus, a mattis mi tempor a. Nunc ex nisi, aliquet sed bibendum vel, varius sit amet tortor. Integer at feugiat tortor.",
    "Html": "\r\n<h1>Ipsum Lorem</h1><p>\r\nLorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean lobortis nunc eget nisi euismod, ac tristique dolor luctus. Praesent consequat euismod tortor semper volutpat. Praesent in dui a lectus bibendum elementum. Etiam facilisis interdum erat, ut pretium leo condimentum eget. Integer maximus gravida nibh in posuere. Sed rhoncus pharetra est, ac consectetur nisl ultrices sed. Quisque in porttitor ligula. Nulla facilisi. Morbi rhoncus sem id accumsan finibus. <br></br>Vivamus pharetra ex id dictum tristique. Sed scelerisque finibus tempor. Maecenas eu luctus neque, eget congue leo. Vivamus sollicitudin malesuada scelerisque. Curabitur erat massa, tincidunt ut congue vitae, tempus sit amet orci. Vivamus varius dolor lobortis, rhoncus dui a, viverra eros. Ut tellus ante, gravida quis lectus et, malesuada auctor metus. Aenean a tincidunt mauris.</p><ul>\r\n<li>Phasellus varius <strong>ligula</strong> sit amet ante elementum.</li>\r\n<li>Ut tellus ante, gravida quis lectus et, malesuada auctor metus. Aenean a tincidunt mauris.</li>\r\n<li>Morbi pharetra posuere orci. Nulla facilisi. Nulla quis nunc nisl.</li>\r\n</ul>\r\n\r\n<p><i>Maecenas molestie tristique lacus, a mattis mi tempor a. Nunc ex nisi, aliquet sed bibendum vel, varius sit amet tortor. Integer at feugiat tortor.</i></p>"
}
```

Allows you to populate the internal or external 'Overview' fields for the given `Company` entity using either HTML or Plain Text.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/profile`

<i>Table 1. Parameters Summary</i>

Parameter | Default | Description
--------- | ------- | -----------
id | [required] | Specify the unique identifier for the desired `Company` entity.
external | [required] | Specify if you wish to update the internal or external 'Overview' for the desired `Company` entity.

# Images
Invenias applications allow their end-users to add, replace and remove images from `Company` and `People` entities. The following endpoints can be leveraged to read, replace and add images to the aforementiond entities.

<aside class="notice">
Please note, when an image is uploaded to an Invenias database the original file we be duplicated and resized into many different resolutions for use with various Invenias applicaitons. 
</aside>

<p><i>Table 1. Image Endpoint Summary</i></p>

Name | Description
---- | -----------
[POST /api/v1/companies/{id}/recordpicture] (https://bullhorn.github.io/invenias-api-docs/#GET-api-v1-companies-id-recordpicture) | Allows you to upload and link an image to a given `Company` entity.
[GET /api/v1/companies/{id}/recordpicture] (https://bullhorn.github.io/invenias-api-docs/#GET-api-v1-companies-id-recordpicture) | Returns a list of URL's for an image linked to a given `Company` entity.
[PUT /api/v1/companies/{id}/recordpicture] (https://bullhorn.github.io/invenias-api-docs/#put-api-v1-companies-id-recordpicture) | Allows you to replace an image liked to a given `Company` entity.
[DELETE /api/v1/companies/{id}/recordpicture] (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-companies-id-recordpicture) | Allows you to delete an image liked to a given `Company` entity.
[POST /api/v1/people/{id}/recordpicture] (https://bullhorn.github.io/invenias-api-docs/#GET-api-v1-people-id-recordpicture) | Allows you to upload and link an image to a given `People` entity.
[GET /api/v1/people/{id}/recordpicture] (https://bullhorn.github.io/invenias-api-docs/#GET-api-v1-people-id-recordpicture) | Returns a list of URL's for an image linked to a given `People` entity.
[PUT /api/v1/people/{id}/recordpicture] (https://bullhorn.github.io/invenias-api-docs/#put-api-v1-people-id-recordpicture) | Allows you to replace an image liked to a given `People` entity.
[DELETE /api/v1/people/{id}/recordpicture] (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-people-id-recordpicture) | Allows you to delete an image liked to a given `People` entity.

## POST /api/v1/companies/{id}/recordpicture
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/companies/{id}/recordpicture' \
--header 'Authorization: Bearer {token}' \
--form 'File=@"/C:/Users/glen.chamberlain/Desktop/cropped-Invenias_Linear.png"'
```
> Please note, successful requests will return a 204 No Content response code.

This endpoint allows you to upload and link an image to a given `Company` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/recordpicture`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for the `Company` entity you wish to upload and link an image to.

## GET /api/v1/companies/{id}/recordpicture
> Example (cURL)
```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/companies/{id}/recordpicture' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
[
    {
        "Uri": "https://{subdomain}.invenias.com/imageBlobs/{id}/{id}?modified=1639402858",
        "MimeType": "PNG"
    },
    {
        "Uri": "https://{subdomain}.invenias.com/imageBlobs/{id}/{id}_16_16?modified=1639402858",
        "Width": 16.0,
        "Height": 16.0,
        "MimeType": "PNG"
    },
    {
        "Uri": "https://{subdomain}.invenias.com/imageBlobs/{id}/{id}_16_16_circle?modified=1639402858",
        "Width": 16.0,
        "Height": 16.0,
        "MimeType": "PNG"
    },
    {
        "Uri": "https://{subdomain}.invenias.com/imageBlobs/{id}/{id}_32_32?modified=1639402858",
        "Width": 32.0,
        "Height": 32.0,
        "MimeType": "PNG"
    }...
]
```
This endpoint returns a list of URL's to the image uploaded for the given `Company` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/recordpicture`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for the `Company` entity you to get the image for.

## PUT /api/v1/companies/{id}/recordpicture
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/companies/{id}/recordpicture' \
--header 'Authorization: Bearer {token}' \
--form 'File=@"/C:/Users/glen.chamberlain/Desktop/Invenias_By_Bullhorn_RGB_Color.png"'
```
> Please note, successful requests will return a 204 No Content response code.

This endpoint allows you to update/replace an image to linked a given `Company` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/recordpicture`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for the `Company` entity you wish to replace/update the image for.

## DELETE /api/v1/companies/{id}/recordpicture
> Example (cURL)
```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/companies/{id}/recordpicture' \
--header 'Authorization: Bearer {token}'
```
> Please note, successful requests will return a 200 OK response code.

This endpoint allows you to delete an image to linked a given `Company` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/recordpicture`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for the `Company` entity you wish to delete the image for.

## POST /api/v1/people/{id}/recordpicture
> Example (cURL)
```shell
curl --location --request POST 'https://{subdomain}.invenias.com/api/v1/people/{id}/recordpicture' \
--header 'Authorization: Bearer {token}' \
--form 'URL=@"/C:/Users/glen.chamberlain/Pictures/PersonImage.jpeg"'
```
> Please note, successful requests will return a 204 No Content response code.

This endpoint allows you to upload and link an image to a given `People` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/people/{id}/recordpicture`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for the `People` entity you wish to upload and link an image to.

## GET /api/v1/people/{id}/recordpicture
> Example (cURL)
```shell
curl --location --request GET 'https://{subdomain}.invenias.com/api/v1/people/{id}/recordpicture' \
--header 'Authorization: Bearer {token}'
```

> Example Response (JSON)
```shell
[
    {
        "Uri": "https://{subdomain}.invenias.com/imageBlobs/a9504f72-4f20-4d4a-a8d1-1a18ed571c9d/a9504f72-4f20-4d4a-a8d1-1a18ed571c9d?modified=1639409889",
        "MimeType": "PNG"
    },
    {
        "Uri": "https://{subdomain}.invenias.com/imageBlobs/a9504f72-4f20-4d4a-a8d1-1a18ed571c9d/a9504f72-4f20-4d4a-a8d1-1a18ed571c9d_16_16?modified=1639409889",
        "Width": 16.0,
        "Height": 16.0,
        "MimeType": "PNG"
    },
    {
        "Uri": "https://{subdomain}.invenias.com/imageBlobs/a9504f72-4f20-4d4a-a8d1-1a18ed571c9d/a9504f72-4f20-4d4a-a8d1-1a18ed571c9d_16_16_circle?modified=1639409889",
        "Width": 16.0,
        "Height": 16.0,
        "MimeType": "PNG"
    }
]
```
This endpoint returns a list of URL's to the image uploaded for the given `People` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/companies/{id}/recordpicture`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for the `People` entity you to get the image for.

## PUT /api/v1/people/{id}/recordpicture
> Example (cURL)
```shell
curl --location --request PUT 'https://{subdomain}.invenias.com/api/v1/people/{id}/recordpicture' \
--header 'Authorization: Bearer {token}' \
--form 'File=@"/C:/Users/glen.chamberlain/Pictures/PersonImage.jpeg"'
```
> Please note, successful requests will return a 204 No Content response code.

This endpoint allows you to update/replace an image to linked a given `People` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/people/{id}/recordpicture`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for the `People` entity you wish to replace/update the image for.

## DELETE /api/v1/people/{id}/recordpicture
> Example (cURL)
```shell
curl --location --request DELETE 'https://{subdomain}.invenias.com/api/v1/people/{id}/recordpicture' \
--header 'Authorization: Bearer {token}'
```
> Please note, successful requests will return a 200 OK response code.

This endpoint allows you to delete an image to linked a given `People` entity.

### HTTP Request
`https://{subdomain}.invenias.com/api/v1/people/{id}/recordpicture`

Parameter | Default | Type | Description
--------- | ------- | ---- | -----------
id | [required] | String | Specify the unique identifier for the `People` entity you wish to delete the image for.

# Documents
<i>Table 1. Document Endpoint Summary</i>

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
curl --location --request POST 'https://{subdomain}.miginvenias.com/api/v1/documents/bulkDelete' \
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

Please note, the number of category lists allowed in Invenias is only limited only by the resources available in the database. This means that it's workable to have thousands of category lists; however, this is not advised as it would be unrealistic for end-users of our products to be categorizing records across this many data points.

Parent-child hierarchies have an unusual way of storing the hierarchy in the sense that they have a variable depth.

In the parent-child pattern, the hierarchy is not defined by columns in the table of the original data source. The hierarchy is based on a structure where each node of the hierarchy is related to the key to its parent node. For example, Figure 2 shows the first few rows of a parent-child hierarchy that defines an Industry structure for Assignments.

<img src="images\categoriesparentchildflat.png" alt="X-Request-Quota-Remaining" class="inline"/>
<br><i>Figure 2. Invenias Professional – Parent Child Pattern</i>
<br></br>

The full expansion of the parent-child hierarchy in this example requires four levels. Figure 3 shows that there is one column for each level of the hierarchy. The number of columns required depends on the data, so it is possible to add additional levels to accommodate for future changes in the data.

<img src="images\categoriesparentchildpivot.png" alt="X-Request-Quota-Remaining" class="inline"/>
<br><i>Figure 3. Example of a flattened hierarchy where each level of the original parent-child hierarchy is stored in a separate column.</i>
<br></br>

<i>Table 1. Category Endpoints Summary</i>

Name | Description
---- | -----------
[POST /api/v1/categorylists]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-categorylists) | Used to create a new Category List.
[POST /api/v1/categorylists/{categoryListId}/entries]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-categorylists-categorylistid-entries) | Used to create a new category within any given Category List
[POST /api/v1/categorylists/{id}/entries/list]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-categorylists-id-entries-list) | Returns a list of all the categories that exist within a specific Category List.
[GET /api/v1/categorylists/{categoryListId}/entries/{entryId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categorylists-categorylistid-entries-entryid) | Returns the details for a specific Category List entry within the specified Category List.
[DELETE /api/v1/categorylists/{categoryListId}/entries/{entryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-categorylists-categorylistid-entries-entryid) | Used to delete a category within a specific Category List.
[GET /api/v1/categorylists/{id}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categorylists-id) | Returns the details related to a specified Category List.
[GET /api/v1/categories/jobpostings]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-jobpostings) | Returns a list of all the Category Lists enabled for `Advertisement` entities (Including Categories).
[GET /api/v1/jobpostings/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-jobpostings-id-categories) | Returns a list on Category List entries that have been relationally linked to a specific `Advertisement` entity.
[POST /api/v1/jobpostings/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-jobpostings-id-categories) | Used to relationally link a specific `Advertisement` entity with one or more categories.
<<<<<<< Updated upstream
[GET /api/v1/jobpostings/{id}/categories/{categoryListId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-jobpostings-id-categories-categoryListId) | This endpoint returns a list of Category List entries within a specific list where they're relationally linked to an `Advertisement` entity.
[DELETE /api/v1/jobpostings/{id}/categories/{categoryListEntryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-jobpostings-id-categories-categoryListEntryId) | This endpoint is used to remove the relationship between an `Advertisement` and a Category List entry.
[GET /api/v1/categories/assignments]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-assignments) | Returns a list of all the Category Lists enabled for `Assignment` entities (Including Categories).
[POST /api/v1/assignments/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-assignments-id-categories) | Used to relationally link a specific `Assignment` entity with one or more categories.
[GET /api/v1/assignments/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-assignments-id-categories) | Returns a list on Category List entries that have been relationally linked to a specific `Assignment` entity.
[GET /api/v1/assignments/{assignmentId}/categories/{categoryListId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-assignments-assignmentId-categories-categoryListId) | This endpoint returns a list of Category List entries within a specific list where they're relationally linked to an `Assignment` entity.
[DELETE /api/v1/assignments/{assignmentId}/categories/{categoryListEntryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-assignments-assignmentId-categories-categoryListEntryId) | This endpoint is used to remove the relationship between an `Assignment` and a Category List entry.
[GET /api/v1/categories/companies]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-companies) | Returns a list of all the Category Lists enabled for `Company` entities (Including Categories).
[POST /api/v1/companies/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-id-categories) | Used to relationally link a specific `Company` entity with one or more categories.
[GET /api/v1/companies/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-companies-id-categories) | Returns a list on Category List entries that have been relationally linked to a specific `Company` entity.
[GET /api/v1/companies/{companyId}/categories/{categoryListId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-companies-personId-categories-categoryListId) | This endpoint returns a list of Category List entries within a specific list where they're relationally linked to a `Company` entity.
[DELETE /api/v1/companies/{companyId}/categories/{categoryListEntryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-companies-companyId-categories-categoryListEntryId) | This endpoint is used to remove the relationship between a `Company` and a Category List entry.
[GET /api/v1/categories/people]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-people) | Returns a list of all the Category Lists enabled for `Person` entities (Including Categories).
[POST /api/v1/people/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-people-id-categories) | Used to relationally link a specific `Person` entity with one or more categories.
[GET /api/v1/people/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-people-id-categories) | Returns a list on Category List entries that have been relationally linked to a specific `Person` entity.
[GET /api/v1/people/{personId}/categories/{categoryListId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-people-personId-categories-categoryListId) | This endpoint returns a list of Category List entries within a specific list where they're relationally linked to a `Person` entity.
[DELETE /api/v1/people/{personId}/categories/{categoryListEntryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-people-personId-categories-categoryListEntryId) | This endpoint is used to remove the relationship between a `Person` and a Category List entry.
[GET /api/v1/categories/programmes]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-programmes) | Returns a list of all the Category Lists enabled for Programme entities (Including Categories).
[POST /api/v1/programmes/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-programmes-id-categories) | Used to relationally link a specific `Programme` entity with one or more categories.
[GET /api/v1/programmes/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-programmes-id-categories) | Returns a list on Category List entries that have been relationally linked to a specific `Programme` entity.
[GET /api/v1/programmes/{programmeId}/categories/{categoryListId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-programmes-programmeId-categories-categoryListId) | This endpoint returns a list of Category List entries within a specific list where they're relationally linked to a `Programme` entity.
[DELETE /api/v1/programmes/{programmeId}/categories/{categoryListEntryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-programmes-programmeId-categories-categoryListEntryId) | This endpoint is used to remove the relationship between a `Programme` and a Category List entry.
[GET /api/v1/categories/assignments/{assignmentsId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-assignments-assignmentsId) | Returns a list of Category Lists & Categories relationally linked to a specific `Assignment` entity.
[GET /api/v1/categories/companies/{companyId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-companies-companyId) | Returns a list of Category Lists & Categories relationally linked to a specific `Company` entity.
[GET /api/v1/categories/people/{personId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-people-personId) | Returns a list of Category Lists & Categories relationally linked to a specific `Person` entity.
[GET /api/v1/categories/programmes/{programmesId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-programmes-programmesId) | Returns a list of Category Lists & Categories relationally linked to a specific `Programme` entity.
=======
[GET /api/v1/jobpostings/{id}/categories/{categoryListId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-jobpostings-id-categories-categorylistid) | This endpoint returns a list of Category List entries within a specific list where they're relationally linked to an `Advertisement` entity.
[DELETE /api/v1/jobpostings/{id}/categories/{categoryListEntryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-jobpostings-id-categories-categorylistentryid) | This endpoint is used to remove the relationship between an `Advertisement` and a Category List entry.
[GET /api/v1/categories/assignments]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-assignments) | Returns a list of all the Category Lists enabled for `Assignment` entities (Including Categories).
[POST /api/v1/assignments/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-assignments-id-categories) | Used to relationally link a specific `Assignment` entity with one or more categories.
[GET /api/v1/assignments/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-assignments-id-categories) | Returns a list on Category List entries that have been relationally linked to a specific `Assignment` entity.
[GET /api/v1/assignments/{assignmentId}/categories/{categoryListId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-assignments-assignmentid-categories-categorylistid) | This endpoint returns a list of Category List entries within a specific list where they're relationally linked to an `Assignment` entity.
[DELETE /api/v1/assignments/{assignmentId}/categories/{categoryListEntryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-assignments-assignmentid-categories-categorylistentryid) | This endpoint is used to remove the relationship between an `Assignment` and a Category List entry.
[GET /api/v1/categories/companies]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-companies) | Returns a list of all the Category Lists enabled for `Company` entities (Including Categories).
[POST /api/v1/companies/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-companies-id-categories) | Used to relationally link a specific `Company` entity with one or more categories.
[GET /api/v1/companies/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-companies-id-categories) | Returns a list on Category List entries that have been relationally linked to a specific `Company` entity.
[GET /api/v1/companies/{companyId}/categories/{categoryListId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-companies-companyid-categories-categorylistid) | This endpoint returns a list of Category List entries within a specific list where they're relationally linked to a `Company` entity.
[DELETE /api/v1/companies/{companyId}/categories/{categoryListEntryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-companies-companyid-categories-categorylistentryid) | This endpoint is used to remove the relationship between a `Company` and a Category List entry.
[GET /api/v1/categories/people]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-people) | Returns a list of all the Category Lists enabled for `Person` entities (Including Categories).
[POST /api/v1/people/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-people-id-categories) | Used to relationally link a specific `Person` entity with one or more categories.
[GET /api/v1/people/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-people-id-categories) | Returns a list on Category List entries that have been relationally linked to a specific `Person` entity.
[GET /api/v1/people/{personId}/categories/{categoryListId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-people-personid-categories-categorylistid) | This endpoint returns a list of Category List entries within a specific list where they're relationally linked to a `Person` entity.
[DELETE /api/v1/people/{personId}/categories/{categoryListEntryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-people-personid-categories-categorylistentryid) | This endpoint is used to remove the relationship between a `Person` and a Category List entry.
[GET /api/v1/categories/programmes]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-programmes) | Returns a list of all the Category Lists enabled for Programme entities (Including Categories).
[POST /api/v1/programmes/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#post-api-v1-programmes-id-categories) | Used to relationally link a specific `Programme` entity with one or more categories.
[GET /api/v1/programmes/{id}/categories]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-programmes-id-categories) | Returns a list on Category List entries that have been relationally linked to a specific `Programme` entity.
[GET /api/v1/programmes/{programmeId}/categories/{categoryListId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-programmes-programmeid-categories-categorylistid) | This endpoint returns a list of Category List entries within a specific list where they're relationally linked to a `Programme` entity.
[DELETE /api/v1/programmes/{programmeId}/categories/{categoryListEntryId}]  (https://bullhorn.github.io/invenias-api-docs/#delete-api-v1-programmes-personid-categories-categorylistentryid) | This endpoint is used to remove the relationship between a `Programme` and a Category List entry.
[GET /api/v1/categories/assignments/{assignmentsId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-assignments-assignmentsid) | Returns a list of Category Lists & Categories relationally linked to a specific `Assignment` entity.
[GET /api/v1/categories/companies/{companyId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-companies-companyid) | Returns a list of Category Lists & Categories relationally linked to a specific `Company` entity.
[GET /api/v1/categories/people/{personId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-people-personid) | Returns a list of Category Lists & Categories relationally linked to a specific `Person` entity.
[GET /api/v1/categories/programmes/{programmesId}]  (https://bullhorn.github.io/invenias-api-docs/#get-api-v1-categories-programmes-programmesid) | Returns a list of Category Lists & Categories relationally linked to a specific `Programme` entity.
>>>>>>> Stashed changes

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
Please note, you can only delete a Category List entry is it's not relationally linked to one or more core entity types (e.g., People, Assignments, Companies, Programmes, etc…). If you wish to delete a Category List entry that is relationally linked to core entity types, you will need to delete all the relations before using this endpoint to delete a Category List entry.
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

This endpoint is used to get Category List entries within a given Category List.

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
Please note, if you're considering developing an integration that creates people in bulk using these endpoints, you must not exceed the maximum number of concurrent requests. For more information, please refer to this article on the Sovren website: [https://docs.sovren.com/Documentation/ResumeParser#batch-parsing-concurrency](https://docs.sovren.com/Documentation/ResumeParser#batch-parsing-concurrency).

Invenias utilizes Sovren, a resume parsing tool, to provide document parsing services to end-users of Invenias applications.

<<<<<<< Updated upstream
A resume parser is a piece of software that can read, understand, and classify all the data on a resume, just like a human can–but much faster.
It's possible for API integrations to leverage Sovren to create and/or update 'Person' entities in Invenias by parsing a source document and ingesting the structured output. However, there is no quick and easy way to create a person by parsing a document. Depending upon the type of information you wish to use, it may require validation across several areas.
=======
A resume parser is a powerful software that can read, comprehend, and categorize all the data on a resume, just like a human can, but much faster.

It's possible for API integrations to leverage Sovren to create and/or update 'Person' entities in Invenias by parsing a source document and ingesting the structured output. However, creating a person by parsing a document is not a quick and easy task and may require validation across multiple areas.

When planning your integration, please consider the following aspects:
>>>>>>> Stashed changes

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
<p>
<img src="images\parsingworkflow.png" alt="X-Request-Quota-Remaining" class="inline"/>
<i>Figure 1. Simple workflow example.</i></p>

<p><i>Table 1. Parsing Endpoints Summary</i></p>

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

Please note, if you set the value for the parameter named 'SaveParsedDocumentAsDefault' to `true` in the request body it will save the parsed source to the `Person` type entities record and flag it as the `Default` CV and write the contents to the 'CV/RESUME' tab in the Person profile pane. This will make the text content of the document searchable when using the 'Advanced Search' feature for `Person` type entities in Invenias Professional application.

<aside class="warning">
Please note, to create (or update) a new Person type entity, you must include the 'NameComponents' array in the request body.
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

Please note, if you set the value for the parameter named 'SaveParsedDocumentAsDefault' to `true` in the request body it will save the parsed source to the `Person` type entities record and flag it as the `Default` CV and write the contents to the 'CV/RESUME' tab replacing the content in the Person profile pane. This will make the text content of the document searchable when using the 'Advanced Search' feature for `Person` type entities in Invenias Professional application.

<aside class="warning">
Please note, to create (or update) a new Person type entity, you must include the 'NameComponents' array in the request body or not.
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
An Application Programming Interface (API) encompasses a collection of routines, protocols, and tools meticulously designed to facilitate the development of software applications. APIs serve as indispensable building blocks that empower developers to seamlessly interact with various software components and systems, enabling the creation of robust and innovative applications.

### What is the rate limit of the API?
We use a fixed-window rate limiting strategy you can make up to `3000` api calls at any interval within a 5 minute window. For more information on rate limits please see [here](https://bullhorn.github.io/invenias-api-docs/#rate-limiting).

### Will, you make changes to your API that will leave my integration unusable?
At Invenias, our goal is to introduce API changes in an additive manner, ensuring existing integrations remain unaffected. We prioritize maintaining backward compatibility to minimize disruptions. However, we acknowledge that occasional situations may arise where changes could impact existing integrations. In such cases, rest assured that we will promptly communicate any necessary adjustments to facilitate a smooth transition. Our commitment is to keep you informed and supported throughout the process.

### Can you help us with the coding?

REST is an industry-standard protocol that operates independently of any specific programming language or technology, relying solely on HTTP. It's important to note that our support team may not be able to assist with coding issues, as they may not have comprehensive knowledge of all web service client implementations or your platform's specific architecture and technology stack. Consequently, Invenias cannot guarantee or provide support for your custom code.

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

Please carefully review every element of the API request, ensuring there are no typos or errors in the endpoint URL, headers (names and values), and request body. If you copied and pasted any part of the API request, double-check for any unintended characters or mistakes that could potentially cause issues during execution. Paying extra attention to these details will help ensure the accuracy and reliability of your API interactions.

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

Please note, if you are receiving `404` or `403` response codes from our servers, there's a chance the Application has expired. In this event please see [here](https://bullhorn.github.io/invenias-api-docs/#renewing-an-application) for more information on what to do. If you're unsure if your application has expired or not please email our support team using inveniassupport@bullhorn.com and they will check for you.

## Not Found (404) Response Code
The `404` error status code indicates that the REST API can't map the client's URI to a resource, but may be available in the future. ... This status code is used when our server does not wish to reveal exactly why the request has been refused, or when no other response applies.

Please note, if you are receiving `404` or `403` response codes from our servers, there's a chance the Application has expired. In this event please see [here](https://bullhorn.github.io/invenias-api-docs/#renewing-an-application) for more information on what to do. If you're unsure if your application has expired or not, please email our support team using inveniassupport@bullhorn.com and they will check for you.

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
