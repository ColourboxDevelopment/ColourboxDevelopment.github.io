---
layout: page
title: "Authentication"
permalink: /authentication/
layout: default
---

# Authentication

To make calls to the API you need to get a token. This token is then used to authorize all subsequent calls to the API. 

If you don't have an API key or secret, please see "Getting Started" 

## Getting a token

To get a token make a `POST` call to `/authenticate/userpasshmac` with the following parameters: 

========   ===========================
username   Username of user 
password   Password for the user
key        API Key
ts         Current Unix timestamp 
hmac       Calculated code (see below)
========   ===========================


`hmac` should be calculated in the following way: 


```
HMAC-SHA1(secret, apiKey + ":" + ts)
```    

Example of how it's done in PHP 

```php
hash_hmac('sha1', $apiKey . ":" . time(), $secret);
```

### Example calculation

Given that the API key is `company2key` , the secret is `company2secret` and the timestamp is `1429187979`, the computed hmac will be: `7e42196943a343e7494fc2636a3f7313edfcd81f`


If authentication is successful the API will return a token as well an expiration date for the token.

## Using the token
Once the token is obtained, it should be added to all subsequent calls in the Authorization Header.

```
Authorization: CBX-SIMPLE-TOKEN Token=<token>
```

Example using curl:

```
curl -H "Authorization: CBX-SIMPLE-TOKEN Token=e8ebf927234ec5dfc4cf66edbbe1382e886142f" https://api.colourbox.com/whoami
```

Running the above curl command will give you back information about your user. 
