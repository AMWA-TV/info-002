# NMOS Security Implementation Guide: Overview
_(c) AMWA 2021, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

## Scope
This document is intended as a guide for implementers who want to add security to their NMOS Nodes and Controllers.

Following feedback from implementers, the focus of this document is currently on implementing NMOS Nodes and Controllers with support for IS-10 and BCP-003-02. However, this is a living document and guidance on the use of other NMOS security specifications will be added as necessary.

## Use of Normative Language
This document is a guide to the AMWA NMOS security specifications only and not a specification itself.
It provides informational guidance around the normative requirements of the relevant specifications.
If there are any inconsistencies between this guide and the specifications then, unequivocally, the specification is correct.
Any normative language key words found in this document are not to be interpreted as RFC 2119 key words.

## Adding Authentication to API Calls
[IS-10](https://specs.amwa.tv/is-10/) authorization is based on OAuth 2.0 and so adding authorization to your IS-04, IS-05, IS-07 and IS-08 API calls simply involves adding an Access Token to the HTTP header of your requests, whether that be a [Node interacting with a Registry](Node%20to%20Registry%20Interactions%20(IS-04).md), or a [Controller interacting with a Registry or Node](Controller%20to%20Registry%20(IS-04)%20and%20Node%20Interactions%20(IS-05%2C%20IS-08).md).
Additional requirements that are specific to particular APIs are defined by [BCP-003-02](https://specs.amwa.tv/bcp-003-02/).

## Getting an Access Token
Both [NMOS Nodes](Node%20to%20Authorization%20Server%20Interactions.md) and [Controllers](Controller%20to%20Authorization%20Server%20Interactions.md) can retrieve Access Tokens from an [Authorization Server](Authorization%20Server%20Setup.md).

## Validating API Calls
If you receive an authenticated API call you will need to check that the [token you've received is valid](Validating%20Access%20Tokens.md).

## Getting Started
There is some [prerequisite knowledge](Prerequisites.md) needed and some suggested [resources](Development%20Resources.md) to help get you started.

## Terminology
The use of OAuth 2.0 terminology has been adopted in the IS-10 specification. OAuth 2.0 was originally developed for allowing web apps to access protected resources, where the web app is the Client and the protected resource is the Resource Server. However, the NMOS Node acts as both a Client (when, for instance, registering with the NMOS Registry) AND as a Resource Server (when, for instance, accepting a connection request). For this reason the Node is sometimes referred to as the Client and sometimes as the Resource Server depending on context.
