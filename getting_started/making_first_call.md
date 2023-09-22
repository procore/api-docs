---
permalink: /making-first-call
title: Making Your First Call to the Procore API
layout: default
section_title: Getting Started
---

## Before You Begin

Prior to making calls to the Procore API you must complete the following tasks:

- [Register for a Developer Account]({{ site.url }}{{ site.baseurl }}{% link getting_started/new_account.md %}) on the Procore Developer Portal.
- [Create an Application]({{ site.url }}{{ site.baseurl }}{% link building_applications/building_apps_intro.md %}) using your Developer Account.

In addition, you will need to familiarize yourself with the [OAuth 2.0 authentication protocol]({{ site.url }}{{ site.baseurl }}{% link oauth/oauth_introduction.md %}) as access to the Procore API is secured by the authorization and authentication requirements of OAuth 2.0.
Applications you develop for integrating with Procore must implement OAuth 2.0.

> HTTPS REQUIREMENT
>
> All Procore API resources are protected by the [Transport Layer Security (TLS) protocol](https://tools.ietf.org/html/rfc5246) for ensuring user interaction with Procore over the internet occurs securely without transmissions being vulnerable to outside entities.
TLS encrypts a channel between two endpoints (for example, between a web browser and web server) to provide privacy and reliability of data transmitted over the communications channel.
As a result, any call you make to a Procore API resource must use the `HTTPS` scheme in the URL.
See [Transport Layer Security Requirements]({{ site.url }}{{ site.baseurl }}{% link api_essentials/tls_reqs.md %}) for additional information.

## cURL and Postman

Two popular web development test tools - cURL and Postman - can be used to explore the capabilities of the Procore API without having to fully build out your application.
In the following sections we use these tools to illustrate how you can make your first call to the Procore API.
If you are unfamiliar with these tools, here are some helpful resources to get you started.

- [cURL Home Page](https://curl.haxx.se/)
- [cURL GitHub Repository Readme](https://github.com/curl/curl/blob/master/README.md)
- [Postman Home Page](https://www.getpostman.com/)
- [Postman Documentation](https://www.getpostman.com/docs/v6/)

### Procore OAuth 2.0 Postman Collection with cURL Examples

We've put together a helpful Postman collection of cURL examples for the Procore OAuth 2.0 Authentication endpoints that you can use while you build, test, and maintain your application.
Simply [visit this link](https://documenter.getpostman.com/view/3996804/SW7bzS65) to view the collection and begin exploring the Procore OAuth 2.0 authentication endpoints.
Each endpoint includes an explanation of its functionality along with a pre-formatted cURL example command that you can copy and paste as needed.

### Using cURL to Make Your First Procore API Call

In this section, the cURL command line tool is used to retrieve an OAuth 2.0 access token and make a simple call to the Procore API.

### 1. Obtain Authorization from the User

The first step to obtaining an access token is to open your browser and make a call to the ‘Authorize’ endpoint using a REST URL.
The syntax for this URL is shown here:

    https://login.procore.com/oauth/authorize?response_type=code&client_id=<CLIENT_ID>&redirect_uri=<REDIRECT_URI>

Let’s break this URL into its constituent components, so we can examine it in more detail:

- The first piece of the URL - `https://login.procore.com/oauth` - is known as the Base URL. We include the Base URL with every call we make to the Procore API.
- Next, we see the endpoint definition itself - `/authorize`.
- Following that, three distinct query parameters are defined - `response_type`, `client_id`, and `redirect_uri`.

A question mark symbol is used to separate the query parameters from the rest of the URL.
Let’s have a look at each of these parameters:

- `response_type` - set to a value of ‘code’, indicates that we want the Procore API `/authorize` endpoint to return an authorization code for us.
- `client_id` - should match what you retrieve from your application page on the Developer Portal.
- `redirect_uri` - should be set to ‘urn:ietf:wg:oauth:2.0:oob’. This allows you to obtain an authorization code without having to run a web server locally

If we build up this URL in the address bar of our browser and send it, the Procore API responds with a panel displaying the returned authorization code.

![auth code]({{ site.baseurl }}/assets/guides/auth-code.png)

It is important to note that the authorization code you obtain is only valid for ten minutes.
As such, you must use this code to retrieve an access token within the 10 minute expiration period.
Otherwise, you will need to call the /authorize endpoint again to obtain a valid authorization code.

### 2. Retrieve an Access Token

Now that we have an authorization code, we can use that to retrieve an access token.
We’ll use the Procore API /token endpoint for this step.
Our cURL command for retrieving an access token will pass the following parameters:

- `client_id` - should match what you retrieve from your application page on the Developer Portal.
- `client_secret` - should match what you retrieve from your application page on the Developer Portal.
- `code` - is the authorization code string you captured in the previous step using the /authorize endpoint.
- `grant_type` - is set to “authorization_code”.
- `redirect_uri` - should be set to “urn:ietf:wg:oauth:2.0:oob” to be consistent with our example.

Below is an example cURL command for retrieving an access token:

```
curl -F grant_type=authorization_code \
  -F client_id=db0d63cfa7ac3ceed7166081542216ec51e36941234e5e879105e36bd76dbf63 \
  -F client_secret=0b57e8d87e35370307ba5f98ad135bd155cabacea56d12344afe083e2eb04b54 \
  -F code=8957b84a67f6ae55ab79c9767836a0af30b7fb7e4c36b27412343728cce71ec7 \
  -F redirect_uri=urn:ietf:wg:oauth:2.0:oob \
  -X POST https://api.procore.com/oauth/token
```

Examining this command we see that we use -F command flags to specify each of the required parameters as being form field data.
In addition, we use backslash characters to denote line breaks which makes the example more readable.
Finally, we use the -X POST flag to tell cURL that we are sending a POST call to the Procore API `/token` endpoint.
Running this command returns a JSON block similar to the following.
Let’s take a look at it’s contents.

```
{
  "access_token":"dbaf9757982a9e738f05d249b7b5b4a266b3a139049317c4909f2f263572c781",
  "token_type":"bearer",
  "expires_in":5400,
  "refresh_token":"76ba4c5c75c96f6087f58a4de10be6c00b29ea1ddc3b2022ee2016d1363e3a7c",
  "created_at":1508271900
}
```

### 3. Making a Call to the Procore API

Now that we have successfully retrieved an access token, we can use it to make our first call to the Procore API.
For this example, we’ll use the simple /me endpoint to show that we can successfully contact the Procore API server and return information about the currently logged in user.
Again, we’ll use cURL to demonstrate this.

First, we’ll build up our cURL command using the following syntax, specifying the authorization code as a header parameter:

    curl -H "Authorization: Bearer <access token>” -X GET https://api.procore.com/rest/v1.0/me

Where `<access token>` is the string value for the access token we retrieved in the previous step.

If we execute this cURL command with a valid access token, it returns a JSON block similar to the following:

```
{
  "id": 1234567,
  "login": "joe.builder@acme.com",
  "name": "Joe Builder"
}
```

You have successfully made your first call to the Procore API!
