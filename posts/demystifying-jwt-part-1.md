---
title: "Demystifying JSON Web Tokens - JWT - Part 1"
date: "2020-07-18"
---

In my current company, I was working on implementing a single sign on (SSO) with a third party ticketing system. There were different authentication strategies we could have used but we went with JWT (JSON Web Tokens). JWT is a token-based authentication system which saw rise in its adoption more when people started moving towards **microservices architecture**. In such services we want to authenticate our APIS but we cannot leverage session-based authentication since both services could be on different servers.

So, JWT (JSON Web Tokens) is similar to other token-based authentication strategies with one difference based on how it is generated. Here the token can be sent in URL, Header or body. It should be **URL-safe string**. An example of a JWT token taken from jwt.io is  

```php
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

Now you would be thinking that where is JSON part here. Hangon for a while, we are reaching there. So, the JWT token has three parts seperated by "." as shown below.

![alt text](../images/jwt.png)

The header part is **base64URL** encoded of the JSON mentioned below:

```js
{
    "typ": "jwt",
    "alg":"HS256"
}
```
Below is the php code to get that hash mentioned above:

```php
$header = json_encode(["alg" => "HS256", "typ" => "JWT"]);
$header_encoded = str_replace('=', '', \strtr(\base64_encode($header), '+/', '-_'));
// eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

The payload/data part. Usually this is where user data is mentioned but it can be any information.

```js
{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022
}
```

Below is the php code to get that hash mentioned above:

```php
$payload = json_encode(["sub" => "1234567890", "name" => "John Doe", "iat" => 1516239022]);
$payload_encoded = str_replace('=', '', \strtr(\base64_encode($payload), '+/', '-_'));
//eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ
```

 > *Here there is one part to note that the header and payload is **base64URL** encoded and not **base64** encoded. The **base64** encoded is not URL-safe. They contain two characters which ("+","/") which makes them unsafe. Thats why in above code you can see we are replacing them with ("-","_") to make them URL-safe.*

 
 
 ### Symmetric JWT Signatures
 Most of times people use signature which is HMAC which uses cryptographic hashing function. Such algorithms are prefixed with **"HS"** as mentioned above (HS256).
 *HS256 is hashing function which implements SHA-256 algorithm.*
 
 This function takes header, payload and a secret key and produces a output as shown below.
 
 

 ![alt text](../images/signature-creation.png)

 Let's see some code in action.

 ```php

 $base64URLencodedHeader = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9";
 $base64URLEncodedPayload = "eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ";
 $string = $base64URLencodedHeader . "." . $base64URLEncodedPayload;
 $signature = hash_hmac('SHA256', $string, $secret, true);
 $signature = str_replace('=', '', \strtr(\base64_encode($signature), '+/', '-_'));
 //SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

 ```

 > Do note that the signature is also **base64URLencoded**



 ![alt text](../images/gen-JWT)



After JWT is recieved at the other end, it is verifed by using **same secret** key and generating the
signature and verifying it with recieved signature. If it gets verified the APIs get authenticated.

Below are illustrations showing that.

 ![alt text](../images/verify-JWT)

 In next part we will see how to generate JWT using asymmetric signatures.














 





















