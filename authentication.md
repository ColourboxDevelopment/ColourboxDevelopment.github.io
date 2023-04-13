---
layout: page
title: "Authentication"
permalink: /authentication/
layout: default
nav_order: 2
---

# Authentication for the API

To make calls to the API, you need a token. This token authorizes all of your API calls. 

If you don't have an API key or an API secret, please see ["Getting Started"](/index.md) before proceeding. 

## Getting an API token

To get a token, make a `POST` call to `/authenticate/userpasshmac` with the following parameters: 

| Parameter        | Description         
| ------------- |-------------
| username    | Username of the user 
| password    | Password for the user
| key    | API Key
| ts    | Current Unix timestamp 
| hmac    | Calculated code (to calculate this code, see below)


`hmac` is a hash code that you calculate as follows: 

```
HMAC-SHA1(secret, apiKey + ":" + ts)
```    

For example, in PHP: 

```php
hash_hmac('sha1', $apiKey . ":" . time(), $secret);
```

### Example hash calculation

Assume that the API key is `company2key` , the secret is `company2secret` and the timestamp is `1429187979`.

The computed hmac will therefore be calculated as: `7e42196943a343e7494fc2636a3f7313edfcd81f`

If authentication is successful, the API will return a token, and an expiration date for the token.

## Using the token
Once you obtain your token, you should add it to all subsequent calls in the Authorization Header.

```
Authorization: CBX-SIMPLE-TOKEN Token=<token>
```

For example, using curl:

```
curl -H "Authorization: CBX-SIMPLE-TOKEN Token=e8ebf927234ec5dfc4cf66edbbe1382e886142f" https://api.colourbox.com/whoami
```

Running the above curl command will return information about your user.