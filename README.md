# Villanova App Engine base Docker images

This repository is providing one image for Wildfly and one image for EAP. Those images are supposed to be inherited by child projects

## Build locally with Apple M1

You can build linux/amd64 images with the help of `buildx` plugin (embedded in Docker Desktop)

````
# Build
docker buildx build --platform linux/amd64 . -t villanova/villanova-wildfly17-base:<version>

# Build and push to local registry
docker buildx build --platform linux/amd64 . -t villanova/villanova-wildfly17-base:<version> --output type=docker

````

# Environment variables
````
:PORTDB_URL: the full JDBC connection string used to connect to the Villanova PORT database
:PORTDB_DATABASE: the name of the Villanova PORT database that is created and hosted in the image
:PORTDB_JNDI: the full JNDI name where the Villanova PORT datasource will be made available to the Villanova Engine JEE application
:PORTDB_DRIVER: the name of the driver for the Villanova PORT database as configured in the JEE application server
:PORTDB_USERNAME: the username of the user that has read/write access to the Villanova PORT database
:PORTDB_PASSWORD: the password of the above-mentioned username.
:PORTDB_SERVICE_HOST: the  name of the server that hosts the Villanova PORT database.
:PORTDB_SERVICE_PORT: the port on the above-mentioned server that serves the Villanova PORT database. Generally we keep to the default port for each RDBMS, e.g. for PostgreSQL it is 5432
:SERVDB_URL: the full JDBC connection string used to connect to the Villanova SERV database
:SERVDB_DATABASE: - the name of the Villanova SERV database that is created and hosted in the image
:SERVDB_JNDI: the full JNDI name where the Villanova SERV datasource will be made available to the Villanova Engine JEE application
:SERVDB_DRIVER: the name of the driver for the Villanova SERV database as configured in the JEE application server
:SERVDB_USERNAME: the username of the user that has read/write access to the Villanova SERV database. For compatibility with mvn jetty:run, please keep this the same as PORTDB_USERNAME
:SERVDB_PASSWORD: the password of the above-mentioned username.  For compatibility with mvn jetty:run, please keep this the same as PORTDB_PASSWORD
:SERVDB_SERVICE_HOST: the  name of the server that hosts the Villanova SERV database
:SERVDB_SERVICE_PORT: the port on the above-mentioned server that serves the Villanova SERV database. Generally we keep to the default port for each RDBMS, e.g. for PostgreSQL it is 5432
:ADMIN_USERNAME: the username of a user that has admin rights on both the SERV and PORT databases. For compatibility with Postgresql, keep this value to 'postgres'
:ADMIN_PASSWORD: the password of the above-mentioned username.
:KIE_SERVER_BASE_URL: The base URL where a KIE Server instance is hosted, e.g. http://villanova-kieserver701.apps.serv.run/
:KIE_SERVER_USERNAME: The username of a user that be used to log into the above-mentioned KIE Server
:KIE_SERVER_PASSWORD: The password of the above-mentioned KIE Server user.
:VILLANOVA_OIDC_ACTIVE: set this variable's value to "true" to activate Villanova's Open ID Connect and the related OAuth authentication infrastructure. If set to "false" all the subsequent OIDC  variables will be ignored. Once activated, you may need to log into Villanova using the following url: <application_base_url>/<lang_code>/<any_public_page_code>.page?username=<MY_USERNAME>&password=<MY_PASSWORD>
:VILLANOVA_OIDC_AUTH_LOCATION: the URL of the authentication service, e.g. the 'login page' that Villanova needs to redirect the user to in order to  allow the OAuth provider to authenticate the user.
:VILLANOVA_OIDC_TOKEN_LOCATION: the URL of the token service where Villanova can retrieve the OAuth token from after authentication
:VILLANOVA_OIDC_CLIENT_ID: the Client ID that uniquely identifies the Villanova App in the OAuth provider's configuration
:VILLANOVA_OIDC_REDIRECT_BASE_URL: the optional base URL, typically the protocol, host and port (https://some.host.com:8080/) that will be prepended to the path segment of the URL requested by the user and provided as a redirect URL to the OAuth provider. If empty, the requested URL will be used as is.
:DOMAIN:  the HTTP URL on which the associated Villanova Engine instance will be served
:CLIENT_SECRET: the secret associated with the 'appbuilder' Oauth Client ID in the Villanova OAuth infrastructure.
:JGROUPS_ENCRYPT_SECRET: - the name of the secret containing the keystore file
:JGROUPS_ENCRYPT_KEYSTORE: - the name of the keystore file within the secret
:JGROUPS_ENCRYPT_NAME: - the name or alias of the kesytore entry containing the server certificate
:JGROUPS_ENCRYPT_PASSWORD: - the password for the keystore and certificate
:JGROUPS_PING_PROTOCOL: - JGroups protocol to use for node discovery. Can be either openshift.DNS_PING or openshift.KUBE_PING.
:JGROUPS_CLUSTER_PASSWORD: -JGroups cluster password
//Ports
:PORT_5000: the port for the NodeJS HTTP Service on images that serve JavaScript applications
:PORT_8080: the port for the HTTP service hosted by JEE Servleit Containers on images that host Java services
:PORT_8443: the port for  the HTTPS service hosted by JEE Servlet Containers that support HTTPS. (P.S. generally we prefer to configure HTTPS on a router such as the Openshift Router)
:PORT_8778: the port for the Jolokia service on JBoss. This service is used primarily for monitoring.
:PORT_8888: the port that a ping service will expose to on support JGroups on images that support JGroups such as the JBoss EAP images

** **PORTDB_DATABASE** - {PORTDB_DATABASE}
** **PORTDB_USERNAME** - {PORTDB_USERNAME}
** **PORTDB_PASSWORD** - {PORTDB_PASSWORD}
** **SERVDB_DATABASE** - {SERVDB_DATABASE}
** **SERVDB_USERNAME** - {SERVDB_USERNAME}
** **SERVDB_PASSWORD** - {SERVDB_PASSWORD}
** **ADMIN_USERNAME** - {ADMIN_USERNAME}
** **ADMIN_PASSWORD** - {ADMIN_PASSWORD}
````