# JWT (JSON Web Tokens)
## Briefing
This guide shows how to create a [JWT](http://https://jwt.io/). JWTs can be signed using a secret (with the HMAC algorithm) or a public/private key pair using RSA. JWT claims can be typically used to pass identity of authenticated users between an identity provider and a service provider, or any other type of claims as required by business processes.

In authentication, when the user successfully logs in using their credentials, a JSON Web Token will be returned and must be saved locally (typically in local storage, but cookies can be also used), instead of the traditional approach of creating a session in the server and returning a cookie.
The following diagram shows all the process:
![link](https://cdn.auth0.com/content/jwt/jwt-diagram.png)

## Creating JWT
JSON Web Tokens consist of three parts separated by dots (.), which are:
* Header
* Payload
* Signature

Therefore, a JWT typically looks like the following:
    ```
    xxxxx.yyyyy.zzzzz
    ```
    
### Step 1. Header
The header component of the JWT contains information about how the JWT signature should be computed. The header is a JSON object in the following format:
```
    {
        "alg": "HS256",
        "typ": "JWT"
    }
```
In this JSON, the value of the “typ” key specifies that the object is a JWT, and the value of the “alg” key specifies which hashing algorithm is being used to create the JWT signature component. In this example, we’re using the HMAC-SHA256 algorithm, a hashing algorithm that uses a secret key, to compute the signature (discussed in more detail in step 3).

### Step 2. Payload

The second part of the token is the payload, which contains the claims. Claims are statements about an entity (typically, the user) and additional metadata. There are three types of claims: registered, public, and private claims.
* Registered claims: These are a set of predefined claims which are not mandatory but recommended, to provide a set of useful, interoperable claims. Some of them are: iss (issuer), exp (expiration time), sub (subject), aud (audience), and others.
*Notice that the claim names are only three characters long as JWT is meant to be compact.*
* Public claims: These can be defined at will by those using JWTs. But to avoid collisions they should be defined in the IANA JSON Web Token Registry or be defined as a URI that contains a collision resistant namespace.
* Private claims: These are the custom claims created to share information between parties that agree on using them and are neither registered or public claims.

An example payload could be:
```
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```
The payload is then Base64Url encoded to form the second part of the JSON Web Token.

### Step 3. Signature
The signature is computed using the following pseudo code:
```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```
What this algorithm does is base64url encodes the header and the payload created in steps 1 and 2. The algorithm then joins the resulting encoded strings together with a period (.) in between them. In this pseudo code, the joined string is assigned to data. To get the JWT signature, the data string is hashed with the secret key using the hashing algorithm specified in the JWT header.

In our example, both the header, and the payload are base64url encoded as:
```
// header
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9
// payload
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9
```
Then, using the joined encoded header and payload, and applying the specified signature algorithm(HS256) on the data string with the secret key set as the string “secret”, we get the following JWT Signature:
```
// signature
TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```
### Step 4. Putting All Three JWT Components Together
Now that all three components are created, we can create the JWT. Remembering the *header.payload.signature* structure of the JWT, we simply need to combine the components, with periods (.) separating them. We use the base64url encoded versions of the header and of the payload, and the signature we arrived at in step 3.
```
// JWT Token
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```
You can try creating your own JWT through your browser at [jwt.io](http://https://jwt.io/).
Going back to the example, the authentication server can now send this JWT to the user.

