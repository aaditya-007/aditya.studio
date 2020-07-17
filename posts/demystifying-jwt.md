---
title: "Demystifying JWT tokens"
date: "2020-07-10"
---

In my current company, I was working on implementing a single sign on (SSO) with a third party ticketing system. There were different authentication strategies we could have used but we went with JWT. JWT is a token-based authentication system which saw rise in its adoption more when people started moving towards **microservices architecture**. In such services we want to authenticate our APIS but we cannot leverage session-based authentication since both services could be on different domains.

So, JWT (JSON Web Tokens) is similar to other token-based authentication strategies with one difference based on how it is generated. Here the token can be sent in URL, Header or body. It should be **URL-safe string**. An example of a JWT token is 

```php
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

Now you would be thinking that where is JSON part here. Hangon for a while, we are reaching there. So, the JWT token has three parts seperated by "." as shown below.

![alt text](../images/jwt.png)

The header part. 

```js
{
    "typ": "jwt",
    "alg":"HS256"
}
```

The payload/data part. Usually this 

```js
{

    "user_id":"1212",
    "email":"aditya@gmail.com"
}

```

The signature part: This part is actually 











