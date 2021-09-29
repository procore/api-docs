---
permalink: /oauth-endpoints
title: Procore API Authentication Endpoints
layout: default
section_title: OAuth 2.0 Authentication
---

## Overview

The Procore API provides a set of authentication endpoints you will use to implement the OAuth 2.0 protocol in your application.
The following sections describe these endpoints along with their request parameters and special considerations.

> PRODUCTION VS. MONTHLY/DEVELOPMENT SANDBOX ENVIRONMENTS
>
> It is important to note that access/refresh tokens are not shared across production and sandbox environments. API calls you make to authenticate with Procore in your production and monthly sandbox environments must use a separate set of authentication keys (client ID and client secret) from those used for your development sandbox environment. In addition, each environment has its own unique base URL for making authentication calls:
>
> - _Production_ - use the `https://login.procore.com/oauth` base URL.
> - _Monthly Sandbox_ - use the `https://login-sandbox-monthly.procore.com/oauth` base URL.
> - _Development Sandbox_ - use the `https://login-sandbox.procore.com` base URL.
>
> Keep in mind that the examples presented below use the production authentication base URL, rather than the sandbox base URLs.

## Grant App Authorization ([/oauth/authorize](https://developers.procore.com/reference/authentication#grant-app-authorization))

The Grant App Authorization endpoint creates and returns either a temporary one-use authorization code with a 10 minute expiration, or an access token depending on the grant type.
This endpoint corresponds to the OAuth 2.0 authorization endpoint described in section 3.1 of the OAuth 2.0 RFC.
Three required request parameters are used with this endpoint as described in in the following table.

| Parameter     |  Description                                                                                                                                                                                                                                                                                            |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| response_type | Specifies the type of response you need from the endpoint. Use a value of ‘code’ if you are using the authorization grant type flow, or use ‘token’ if you are using the implicit grant type flow.                                                                                                      |
| client_id     | Specifies the Client ID value you were issued when you registered your application on the Developer Portal.                                                                                                                                                                                             |
| redirect_uri  | Specifies the URI that the user will be redirected to after they grant authorization to your application. For web applications applications, use an SSL (`https://`) web address. For "server-less" applications (such as cron jobs, backend scripts, etc.) use a value of `urn:ietf:wg:oauth:2.0:oob`. |
| state         | An alphanumeric value used by the client to maintain state between the request and callback. The authorization server includes this value when redirecting the user-agent back to the client. The parameter SHOULD be used for preventing cross-site request forgery                                    |

### How to Use

Unlike the other endpoints, the Grant App Authorization endpoint requires user interaction. Please follow the steps outlined in Steps 1 and Steps 2 in [OAuth 2.0 for Installed Applications]({{ site.url }}{{ site.baseurl }}{{ % link oauth/oauth-endpoints}}) 

## Get Or Refresh an Access Token ([/oauth/token](https://developers.procore.com/reference/authentication#get-or-refresh-an-access-token))

The Get or Refresh an Access Token endpoint retrieves a new access token or refreshes an existing access token.
Certain parameter combinations and values are used depending on which scenario you are handling.
This endpoint corresponds to the token endpoint described in section 3.2 of the OAuth 2.0 RFC.
Note that client-side (JavaScript) applications are not able to use this endpoint to request access tokens or refresh tokens as it requires using the client_secret parameter which is not feasible with client-side applications.
Six request parameters are used with this endpoint as described in the following table.

| Parameter     |  Description                                                                                                                                                                                                                                                                                     |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| grant_type    | Specifies whether you want to retrieve a new access or refresh an existing access token. Use the value `authorization_code` when getting a new access token. Use `refresh_token` when refreshing an existing access token.                                                                       |
| client_id     | Specifies the Client ID value you were issued when you registered your application on the Developer Portal.                                                                                                                                                                                      |
| client_secret | Specifies the Client Secret value you were issued when you registered your application on the Developer Portal.                                                                                                                                                                                  |
| code          | Specifies the value of the `authorization_code` retrieved from the `/oauth/authorize` call. This parameter is only required when getting a new access code.                                                                                                                                      |
| redirect_uri  | Specifies the URI that the user will be redirected to after they grant authorization to your application. For web applications applications, use a `https://` web address. For "server-less" applications (such as cron jobs, backend scripts, etc.) use a value of `urn:ietf:wg:oauth:2.0:oob`. |
| refresh_token | Specifies the refresh token string. This parameter is only required when refreshing an access token.                                                                                                                                                                                             |

cURL Example:

```
curl -F grant_type=authorization_code \
-F client_id=9b36d8c0db59eff5038aea7a417d64eg347a75b41aac771816d2ef1b3109cc2f \
-F client_secret=d6ea27703957b69939b8104ed1234567e210cd2e79af587744a7eb6e58f5b3d2 \
-F code=fd0847dbb559752d932dd3c1ac34ff98d27b11fe2fea5a864f44740cd7919ad0 \
-F redirect_uri=urn:ietf:wg:oauth:2.0:oob \
-X POST https://login.procore.com/oauth/token
```

## Get Token Info ([/oauth/token/info](https://developers.procore.com/reference/authentication#get-token-info))

The Get Token Info endpoint returns details on a specified access token.
The request must contain the access token in the Authorization header in this format:

```
Authorization: Bearer <YOUR_ACCESS_TOKEN>
```

cURL Example:

```
curl -H "Authorization: Bearer 53cff8f4a549beb1c38704158b0f6608a2382f094b6947ecc35c2eed4146a17c" \
-X GET https://login.procore.com/oauth/token/info
```

## Revoke Token ([/oauth/revoke](https://developers.procore.com/reference/authentication#revoke-token))

*IMPORTANT*: For security reasons, it is imperative that you implement a solution within your application or integration for revoking access tokens for Procore user accounts that become inactive or are otherwise terminated.

The Revoke Token endpoint revokes authorization of an access token.
The request must contain the access token and other required parameters in the request body as form-data in order to hide potentially sensitive information, thereby providing an extra layer of security.
If the token is revoked successfully, or if the client submits an invalid token, the authorization server responds with HTTP status code 200.
Note that the Revoke Token endpoint revokes both the Access Token and Refresh Token.
Three request parameters are used with this endpoint as described in the following table.

| Parameter     |  Description                                                                                                                                                                                                                                                                                             |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| token         | Specifies the value of the access token you want to revoke.                                                                                                                                                                                                                                              |
| client_id     | Specifies the Client ID value you were issued when you registered your application on the Developer Portal.                                                                                                                                                                                              |
| client_secret | Specifies the Client Secret value you were issued when you registered your application on the Developer Portal. Note that client_secret is only required for confidential applications. Public applications using the Implicit Grant flow do not need to provide this parameter to revoke access tokens. |

To further understand how the Revoke Token endpoint functions, we recommend becoming familier with how access to Connected Apps is revoked using the Procore Web user interface.
See the [Revoke Access to Connected Apps](http://support.procore.com/products/online/user-guide/company-level/portfolio/tutorials/revoke-access-for-connected-apps) article on our Support Site for additional information.

cURL Example:

```
curl -F token=53cff8f4a549beb1c38704158b0f6608a2382f094b6947ecc35c2eed4146a17c \
 -F client_id=53cff8f4a549beb1c38704158b0f6608a2382f094b6947ecc35c2eed4146a17c \
 -F client_secret=fjeh76f4a549beb1c38704158b0f6608a2382f0bhw346sh8935c2eed4146a17c \
 -X POST https://login.procore.com/oauth/revoke
```
