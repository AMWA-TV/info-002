# JSON Web Tokens (JWT)
_(c) AMWA 2021, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

## Access Tokens

The format of the [Access Token](https://specs.amwa.tv/is-10/releases/v1.0.0/docs/4.4._Behaviour_-_Access_Tokens.html) used by IS-10 is the JSON Web Tokens (JWT). JWT is an open industry standard method for representing claims securely between services (RFC 7519). The claims in a JWT are encoded as a JSON object that is used as the payload of a JSON Web Signature (JWS) structure, enabling the claims to be digitally signed with `RSASSA-PKCS1-v1_5 using SHA-512`.

The Access Token consists of three segments, each being encoded separately using [Base64url Encoding](https://en.wikipedia.org/wiki/Base64#URL_applications "Base64"), and concatenated using periods to produce the JWT:

    base64urlEncoding(header) + '.' + base64urlEncoding(payload) + '.' + base64urlEncoding(signature)

Example of a JWT:

    eyJhbGciOiJSUzUxMiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE2MTI3OTM1MTYsImlhdCI6MTYxMjc5MzMzNiwianRpIjoiMGYxNjc4MzMtOTlkMC00Mjg0LTk5ODAtM2UyYjJlODdmN2RiIiwiaXNzIjoiaHR0cHM6Ly9hdXRob3JpemF0aW9uLXNlcnZlci5jb20iLCJhdWQiOiJyZWdpc3RyeS5leGFtcGxlLm9yZyIsInN1YiI6ImJhZjQxNGU4LTM0YjctNDUwMy1hODdlLTNiNGViOTE2Y2UzYSIsInR5cCI6IkJlYXJlciIsImF6cCI6IjMwYmFmNjE3LTE3NDQtNGMyNC05YmFmLTIyMzUxYmVjMWE3MyIsInNlc3Npb25fc3RhdGUiOiJkMjNmMzE5ZS03OWYzLTRiY2UtOTk1Ni1iNTNlODMzMTUzYzMiLCJhY3IiOiIxIiwic2NvcGUiOiJyZWdpc3RyYXRpb24iLCJ4LW5tb3MtcmVnaXN0cmF0aW9uIjp7InJlYWQiOlsiKiJdLCJ3cml0ZSI6WyIqIl19LCJjbGllbnRJZCI6IjMwYmFmNjE3LTE3NDQtNGMyNC05YmFmLTIyMzUxYmVjMWE3MyJ9.edE5qHXBBGPMZ8S_RZYhW4sYo5uc1GT1gt8z3HkkJwqH_iHL6nD-84l6NOFYpXrE42WjElRyTU5JlPu7gpLMqZHmeqSSDpK-SfF5q25kNHmaiboA5DXQxDeb24ULFztRIjJGmBmWQ6Pg8a8tqVNmO0M75cQBV5pQyDZK1mekFapm3CmzXc628MbQ6YfbeBnNihkbij9NlMFFUffjug12iqiNCv2EEYPSGjsAk2UmVn0WkUQ742EKfqdp1HoBgyMsYJ-D0BjxgRIHWLuYyu7CmrFlOcN2PGbisp_RuGYy3xibgVa-MS8G0_VkQc9rvqkoDKL9kCLPOTyQBeuZsY6wUA
	
JWTs can be straightforwardly introspected using tools such as the [JWT.io debugger](https://jwt.io/#debugger-io).

### Header

The Header is used to identify which algorithm was used to generate the Signature. In accordance with [IS-10](https://specs.amwa.tv/is-10/releases/v1.0.0/docs/4.4._Behaviour_-_Access_Tokens.html), the `alg` field in the header is set to RS512.

```json
{
  "alg": "RS512",
  "typ": "JWT"
}
```

### Payload

The Payload contains the token claims, including when the token became active, when it is due to expire, who the token is intended for, and some other registered (standard) and private claims.

#### Registered Claims

The [IS-10 specification](https://specs.amwa.tv/is-10/releases/v1.0.0/docs/4.4._Behaviour_-_Access_Tokens.html#registered-claims) gives details of how NMOS API Clients and Servers use registered claims.

#### Private Claims

IS-10 has defined some private claims which are used to control read/write access to APIs. These are configured in the Authorization Server and are covered in more detail in [that section](Authorization%20Server%20Setup.md#private-claims).

### Signature

The Signature is created by encoding the Header and Payload using Base64url encoding, then run through the cryptographic algorithm specified in the Header with the issuer private key.
