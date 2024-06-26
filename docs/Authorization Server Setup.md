# Authorization Server Setup
_(c) AMWA 2021, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

IS-10 is all about authorization, so you will need an Authorization Server from which you request Access Tokens. Here you will find some detail on setting up an Authorization Server to provide the correct 'flavor' of Access Tokens to your NMOS Nodes.

## Overview
IS-10 will work with any OAuth 2.0 or OpenID compliant Authorization Server. 
For the purposes of this guide we have used Keycloak which is an open source Identity and Access Management solution.
However, other Authorization Servers are available, so simply map these configuration steps to your current, favorite Auth Server.

## Keycloak Setup

### Install Keycloak
You can get a latest version from the [Keycloak website](https://www.keycloak.org/guides#getting-started).

### Configure Realms
- You need to set up a realm to work within (or use the default one for testing).
- In Realm Settings, go to Tokens and select RS512 from the default signature algorithm.

### Client Scope Config
- Click Client Scopes and configure one or more [client scopes](https://specs.amwa.tv/is-10/releases/v1.0.0/docs/4.4._Behaviour_-_Access_Tokens.html#registered-claims). For example, create one with a name 'registration' (for the IS-04 Registration API), and a protocol of 'openid-connect'.

### Audience Config
- Within the created client scope, go to the 'Mappers' tab. Add a mapper with a name you choose, with a type of 'Audience'. Make the value of this claim match the intended value of the ['aud' claim](https://specs.amwa.tv/is-10/releases/v1.0.0/docs/4.4._Behaviour_-_Access_Tokens.html#aud).
- In order to overcome the form validation, if the value includes a '*', copy and paste it into the field. If you need multiple audience values (using the array format), create multiple 'Audience' type mappers with different values.

### Private Claims
- IS-10 has [defined some private claims](https://specs.amwa.tv/is-10/releases/v1.0.0/docs/4.4._Behaviour_-_Access_Tokens.html#private-claims) which are used to control read/write access to APIs.
- Within the 'Mappers' tab, add another mapper with a name you choose, but with a type of 'Hardcoded Claim'. Give it a Claim JSON Type of 'JSON', a Claim Name of 'x-nmos-\<api\>' (where '\<api\>' is the API/scope name such as 'registration') and a Claim Value which matches the structure in [the specification](https://specs.amwa.tv/is-10/releases/v1.0.0/docs/4.4._Behaviour_-_Access_Tokens.html#example-x-nmos--claims).
- To overcome the validation this needs to be copied and pasted into the field.

### Repeat for All Scopes
- Repeat the previous three steps for each 'scope' supported. Note that in this limited implementation case you can use the same Claim Value for all 'x-nmos-\<api\>' claims, covering the full range of supported scopes. This is hacky, but provides a simpler proof of concept than the alternative [script provider mechanism](https://www.keycloak.org/docs/latest/server_development/#_script_providers).
- In Client Scopes, click the Default Client Scopes tab. Move all of the default and optional scopes which are already on the right hand side (enabled) to the left (disabled). Then move all of the created NMOS scopes from the 'Optional Client Scopes' available list, into the 'Assigned Optional Client Scopes' list.

### Set Up Trusted Hosts
- In Realm Settings, go to Client Registration and Client Registration Policies. Under 'Anonymous Access Policies', enter 'Trusted Hosts' and add '\*.workshop.nmos.tv' (or similar).
- Go to the 'Clients' menu and edit the 'admin-cli' client. Under Client Scopes, add the newly defined scope(s) above to the 'Optional client scopes' list. This is useful to enable the debug procedure below.
- Next, ensure that Keycloak trusts the certificate authority which is in use. This can be achieved by following [Keycloak's instructions](https://www.keycloak.org/server/keycloak-truststore), or by adding the certificate to the default Java keystore using a command like the following, before restarting Keycloak.

```
keytool -import -alias nmosca -file cert.pem -cacerts -storepass changeit
```

### Enable TLS, Redirects and Discovery
- In order to enable Keycloak for TLS (HTTPS) we would suggest using a Reverse Proxy such as the Apache web server.
- As Keycloak is an OpenID Connect server, it places the Client Metadata at a different path to the OAuth 2.0 specification. When using an Apache Reverse Proxy this can be overcome by adding an alias for this path.
- Finally, once Keycloak is set up, suitable DNS records will need to be added to your DNS server in order to enable NMOS discovery.

An example Apache Reverse Proxy site configuration with a metadata alias is shown below. This makes Keycloak available on port 443, forwarding it from port 8082:

```
<VirtualHost _default_:443>
        <Location />
                ProxyPreserveHost On
                ProxyPass https://127.0.0.1:8082/ timeout=30 connectiontimeout=1 max=10 ttl=1 smax=10
                ProxyPassReverse https://127.0.0.1:8082/
        </Location>

        <Location /.well-known/oauth-authorization-server>
                ProxyPreserveHost On
                ProxyPass https://127.0.0.1:8082/auth/realms/master/.well-known/openid-configuration timeout=30 connectiontimeout=1 max=10 ttl=1 smax=10
                ProxyPassReverse https://127.0.0.1:8082/auth/realms/master/.well-known/openid-configuration
        </Location>

        RequestHeader set X-Forwarded-Proto "https"
        RequestHeader set X-Forwarded-Port "443"
        Header set Access-Control-Allow-Origin "*"

        SSLEngine on
        SSLProxyEngine on
        SSLProxyCheckPeerCN off
        SSLProxyCheckPeerName off
        SSLCertificateFile     /etc/ssl/certs/ssl-cert-snakeoil.pem
        SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key
</VirtualHost>
```

Note that to get the Keycloak server to be able to resolve the client addresses / hostnames, then the Keycloak configuration needs to be changed.
Details of how to set up a reverse proxy with Keycloak can be [found here](https://www.keycloak.org/server/reverseproxy).
Without this change, any attempt to register a client as a "Trusted Host" will fail.
