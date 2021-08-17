# Implementing Authenticated API Calls
_(c) AMWA 2021, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

## Overview
This section details how to add API security to your NMOS Node and Controller implementations.

### [Node to Authorization Server Interactions](Node%20to%20Authorization%20Server%20Interactions.md)
This section covers:
- Registering your Node as a Client of the Authorization Server
- Obtaining Access Tokens for authenticated API calls

### [Node to Registry Interactions (IS-04)](Node%20to%20Registry%20Interactions%20(IS-04).md)
This section covers:
- Authenticated registration of a Node with the NMOS Registry

### [Controller to Authorization Server Interactions](Controller%20to%20Authorization%20Server%20Interactions.md)
This section covers:
- Registering your Controller as a Client of the Authorization Server
- Obtaining Access Tokens for authenticated API calls

### [Controller to Registry (IS-04) and Node Interactions (IS-05, IS-08)](Controller%20to%20Registry%20(IS-04)%20and%20Node%20Interactions%20(IS-05%2C%20IS-08).md)
This section covers:
- Authenticated querying of the Registry
- Authenticated connection management
- Authenticated audio channel mapping

### [Event and Tally Interactions (IS-07)](Event%20and%20Tally%20Interactions%20(IS-07).md)
This section covers:
- Authenticated WebSocket connections

### [Validating Access Tokens](Validating%20Access%20Tokens.md)
This section covers:
- Validation of a received Access Token
