# JWT (JSON Web Tokens)
## Briefing
This guide shows how to create a [JWT](http://https://jwt.io/). JWTs can be signed using a secret (with the HMAC algorithm) or a public/private key pair using RSA. JWT claims can be typically used to pass identity of authenticated users between an identity provider and a service provider, or any other type of claims as required by business processes.

The usage of a JWT (for 3 entities) is shown in the diagram below. 
![link](https://cdn-images-1.medium.com/max/1000/1*SSXUQJ1dWjiUrDoKaaiGLA.png)
The entities in this example are the user, the application server, and the authentication server. The authentication server will provide the JWT to the user. With the JWT, the user can then safely communicate with the application.

## Creating JWT
JSON Web Tokens consist of three parts separated by dots (.), which are:
* Header
* Payload
* Signature
Therefore, a JWT typically looks like the following.
    xxxxx.yyyyy.zzzzz
    
###### Header
The header component of the JWT contains information about how the JWT signature should be computed. The header is a JSON object in the following format:
    {
        "alg": "HS256",
        "typ": "JWT"
    }




