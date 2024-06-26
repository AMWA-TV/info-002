# Validating Access Tokens
_(c) AMWA 2021, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

There are a number of steps that need to be considered while [validating the access token](https://specs.amwa.tv/is-10/releases/v1.0.0/docs/4.5._Behaviour_-_Resource_Servers.html#validation-of-access-token).

- Check that the JWT is well-formed, such as JWT contains three segments, Header, Payload and Signature, separated by period ('.') characters. Each segment is Base64url encoded.
- Verify the Signature, using the issuer's public key, to ensure the token has not been tampered with.
- In event the issuer [public key](https://specs.amwa.tv/is-10/releases/v1.0.0/docs/4.5._Behaviour_-_Resource_Servers.html#public-keys) is not known, respond with an HTTP `503` (Service Unavailable) code in order to avoid blocking the incoming authorized request, and fetch the missing public key from the server metadata's `jwks_uri` endpoint.  The server metadata can be retrieved via the token `iss` claim as specified in [RFC 8414](https://tools.ietf.org/html/rfc8414 "OAuth 2.0 Authorization Server Metadata") section 3.
- Check the registered claims, such as token expiration `exp` and token audience `aud`.
- Check the private claims, to verify whether it has permission for accessing the API.

If any of these steps fail, then the associated request is not valid.

[JWT.io](https://jwt.io/) has provided a number of third-party libraries which support the JWT validation for most of the steps described above. However, even using these libraries, the extra step for private claims validation is still necessary for checking the NMOS private claims.
