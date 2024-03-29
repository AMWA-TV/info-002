# Prerequisites
_(c) AMWA 2021, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

Before using this guide, you need to make yourself familiar with the following specifications and technologies.

## NMOS Interface Specifications

### [IS-04 Discovery & Registration](https://specs.amwa.tv/is-04/) and [IS-05 Device Connection Management](https://specs.amwa.tv/is-05/)
As well as a familiarity with the specifications, it is also assumed that you already have a working implementation of IS-04 and IS-05 for your NMOS Node.

### [IS-07 Event & Tally](https://specs.amwa.tv/is-07/)
If your NMOS Node implements the Events API then it is assumed that you are familiar with the specification and have a working implementation of IS-07 for your NMOS Node.

### [IS-08 Audio Channel Mapping](https://specs.amwa.tv/is-08/)
If your NMOS Node implements the Channel Mapping API then it is assumed that you are familiar with the specification and have a working implementation of IS-08 for your NMOS Node.

### [IS-10 Authorization](https://specs.amwa.tv/is-10/)
It's the reason you're here! This guide will describe implementation of IS-10 in NMOS Nodes and Controllers for IS-04, IS-05, IS-07 and IS-08 API calls, so some familiarity with this specification is assumed.

## NMOS Best Current Practices

### [BCP-003-02 Authorization in NMOS Systems](https://specs.amwa.tv/bcp-003-02/)
This document describes how to implement a general OAuth 2.0 authorization system, in line with current best practices. This document outlines additional requirements placed upon Resource Servers and Clients which implement or interact with specific NMOS Interface Specifications.

## OAuth 2.0
Given that IS-10 is based on OAuth 2.0, a familiarity with the way OAuth 2.0 authorization works is needed. The authorization flows used by IS-10 are the Client Credentials Grant and the Authentication Code Flow. The former is generally implemented by NMOS Nodes, and the latter is implemented by NMOS Controllers.

The following four part series of articles gives a good grounding in OAuth 2.0; the first three give an overview of OAuth 2.0 and JWT independent of implementation language. The fourth article is specific to Java implementations.

- [Deep Dive Into OAuth 2.0 and JWT (Part 1 Setting the
   Stage)](https://dzone.com/articles/deep-dive-to-oauth20-amp-jwt-part-1-setting-the-st
   "https://dzone.com/articles/deep-dive-to-oauth20-amp-jwt-part-1-setting-the-st")
- [Deep Dive Into OAuth 2.0 and JWT (Part 2
   OAuth2.0)](https://dzone.com/articles/deep-dive-to-oauth20-amp-jwt-part-2-oauth20)
- [Deep Dive to OAuth 2.0 and JWT (Part
   3)](https://dzone.com/articles/deep-dive-to-oauth20-amp-jwt-part-3-jwt)
- [Deep Dive to OAuth 2.0 and JWT (Part 4 JWT Use
   Case)](https://dzone.com/articles/what-is-zuul)

## JSON Web Tokens (JWT)
JSON Web Tokens are the preferred type of token used in IS-10. A familiarity with JWT can be gained from the articles linked in the [OAuth 2.0 section](Prerequisites.md#oauth-20) above, from this [introduction to JWT](https://jwt.io/introduction) and in the [next section](JSON%20Web%20Tokens%20(JWT).md).
