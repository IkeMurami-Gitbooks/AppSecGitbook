# Разница между OAuth и OpenID

Разница между OAuth и OpenID:

tl;dr

```
Most security issues are with implementation and not protocol, the simpler the better.
SAML/WS-Federation and OpenID Connect all use cryptographically signed tokens that support optional encryption
SAML/WS-Fed is XML based and takes on the XML threat model while OpenID Connect is JSON based {} and takes on the OAuth2 threat model
OpenID Connect provides the authentication layer for OAuth2 and addresses some of the most important security gaps with OAuth2
OpenID Connect when properly implemented and used can be just as secure and SAML/WS-Fed OpenID Connect is a "modern" protocol and well suited for newer use case such as devices and native mobile apps.
```

SAML is a protocol for flows like Web SSO as well as a format for an XML based token (assertion) that is protected via a XML Digital Signature (XML DSIG) and optionally XML encryption (XML ENC). The protocol supports several bindings including SOAP and HTTP (redirect/post)

WS-Federation is a lot more complex in that its actually based on a large set of WS-\* standards such as WS-Trust & WS-security that are SOAP based. WS-Federation is agnostic to the token format as it was designed to be a protocol to negotiate tokens (aka Security Token Service). It's common to use SAML format tokens with WS-Federation, but you could technically also use something like a custom token or even a JWT! There is also a "passive" flow for browser based scenarios that is very similar to HTTP redirect binding for the SAML Authentication Request Protocol

OAuth2 is NOT and authentication protocol but rather an authorization delegation protocol. It defines a set of flows (grant types) to obtain tokens but doesn't define the format of a token. Different flows are specified to accommodate the needs of different applications (clients in OAuth) such as a browser, a background server daemon application, or a native mobile app.

OpenID Connect is built on-top of OAuth2 and provides the authentication layer. It adds a new token to OAuth (id\_token) that is JWT based and set of mandatory params and claims for the protocol and token (assertion). OpenID Connect was designed to be the "modern authentication" answer to most of the SAML/WS-Fed use cases without the XML & SOAP based overhead for modern apps such as native mobile apps and devices.

When comparing SAML/WS-Fed to OpenID Connect (remember vanilla OAuth2 is NOT an authentication protocol), we need to consider the security of the protocol and security of the token (assertion) but also the software "implementation" of both. More complexity to specifications often leads to more developer bugs!

SAML has common XML threats (www.owasp.org/index.php/SAML\_Security\_Cheat\_Sheet) and puts a lot of complexity on both the IdP and SP to get right from as security perspective. SAML for Web SSO with HTTP binding (most common) depends on TLS for transport security to ensure the token is not intercepted. SAML Assertions are commonly issued as a "Bearer Token" which means that there is not way to bind the issued token to the client that presents the token. If you have the assertion that you are presumed to be the subject of the token. The Identity Provider (IdP) acts as the Authentication Service and issues a SAML assertions (token) that is returned to the pre-configured destination known as the service provider (SP).

The request/response messages can use HTTP redirects or Form POST depending on IdP/SP configuration. A successful response form the IdP contains a protocol envelope message with metadata and an assertion (token). The response and/or the assertion needs to digitally signed with XML DSIG by the IdP using it's private key and optionally encrypted with the SP's public key.

The request/response protocol is very minimal but its important to note that the actual user authentication happens over the browser typically with HTML/JS and is not defined the protocol (yes there is a binding that can use HTTP Basic Auth but its not really used in most deployments). The requesting application is typically not authenticated so the most significant threat from a protocol perspective is to ensure that the IdP only delivers the response to the configured SP and not an attacker. SAML Assertions are commonly issued as a "Bearer Token" which means that there is not way to bind the issued token to the client that presents the token. If you have the assertion that you are presumed to be the subject of the token. The location where the assertion is delivered via redirect or POST URL is typically whitelisted in the IdP to ensure that it can only be delivered to "trusted locations" (assuming you trust DNS, TLS, CA's, and your browser!).

WS-Federation as mentioned before is a lot more complex. The active RPC-like SOAP protocol can optionally use any number of features from the wS-\* stack such as message or transport security and bearer or proof-of-possession tokens (where the party presenting the token must cryptographically prove they are the subject of the token). The passive flow is similar to SAML from a security perspective.

Most security issues with SAML/WS-Federation have to deal with the dependency on XML and XML Security (signature & encryption). These are very complicated technologies to get right and most developers only test happy path scenarios. There are so many ways one can implement parsing and data validation that leaves room for an attacker to exploit. A common attack is XML signature wrapping (www.ws-attacks.org/XML\_Signature\_Wrapping).

OpenID Connect (OIDC) is based on OAuth2 and takes on most of the well documented OAuth 2.0 Threat Model and Security Considerations (rfc6819). There has also been a recent Comprehensive Formal Security Analysis of OAuth 2.0 ([https://arxiv.org/abs/1601.01229](https://arxiv.org/abs/1601.01229)). OIDC adds an additional security layer to OAuth2 by returning a signed JWT token to the requesting application called the id token ([https://openid.net/2016/07/16/preventing-mix-up-attacks-with-openid-connect/](https://openid.net/2016/07/16/preventing-mix-up-attacks-with-openid-connect/)). Just like SAML with HTTP binding it also relies on TLS for transport security and takes on the typical DNS, TLS, CA's threat model. JWTs can be signed with shared secret or public/private key HMAC and optionally encrypted. The JSON Web Signature (rfc7515) and Encryption (rfc7516) are a lot more simple than their XML DSIG/ENC counterparts which hopefully means more secure implementations but there have been known issues with common libraries (www.chosenplaintext.ca/2015/03/31/jwt-algorithm-confusion.html). OAuth2 requires on client registration which provides identity for the requesting application which can be public or confidential and require client authentication. User authentication is performed over the browser just like SAML.

There is a lot more inputs to validate with OAuth2 than there is with SAML! Most common issues deal with input validation and binding request/response params to the client. There has been a lot of recent work in the OAuth working group to define best practices for native apps (draft-ietf-oauth-native-apps-06). There is a recent draft in OAuth2 working group to identify additional open security topics (draft-lodderstedt-oauth-security-topics-00).
