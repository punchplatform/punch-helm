# Punch Artifacts Server

The Punch Artifacts Server includes a Restful API and a UI to manage artifacts like parsers, pex, custom node, or generic binaries.

## Configuration

### Kubernetes configuration

#### Image

| Configuration        | Description                         | Default value                                               |
|----------------------|-------------------------------------|-------------------------------------------------------------|
| image.name           | The Artifacts docker image          | ghcr.io/punchplatform/artifacts-server:7.0.1-rc5 |
| image.policy         | The Kubernetes image pull policy    | IfNotPresent                                                |
| image.secret         | Secret name to pull docker image    | admin-secret                                                          |

#### Ingress

| Configuration        | Description                        | Default value  |
|----------------------|------------------------------------|----------------|
| ingress.enabled      | Enable Artifacts access from outside | true          |
| ingress.url          | External URL of the artifacts        |  artifacts.dpsc| 

### Artifacts configuration

#### Server

```yaml
server:
  port: 4245


web:
  enabled: true

application:
    tenant:
      name: mytenant
#    roles:
#      ROLE_USER:
#        - UPLOAD_ARTIFACTS
#      ROLE_ADMIN:
#        - UPLOAD_ARTIFACTS
#        - DELETE_ARTIFACTS
```

| Configuration        | Description                        | Default value  |
|----------------------|------------------------------------|----------------|
| server.port          | Port of the Restful API server     | 4245          |
| web.enabled          | Enable UI                          | true          |
| application.tenant.name          | Tenant to use, used as index prefix for ES and bucket for S3    | mytenant   |
| application.roles          | Role to use see OAUTH section bellow    |    |
| application.file_size          | Maximum artifact size    |    |

#### Metadata

##### Elasticsearch metadata

You can use Elasticsearch backend for metadata.

```yaml
metadata:
  elasticsearch:
    hosts:
      - http://elasticsearch.doc-store:9200
    index_name: artifacts-metadata
    credentials:
      user: <user>
      password: <password>
      # token: <token>
      # token_type: <token_type>
    # ssl_enabled: true
    # ssl_hostname_verification: false
    # ssl_insecure: false
    # You can uses same SSL/TLS configuration like ElasticOutput/ElasticInput nodes, and ES metrics
```

| Configuration        | Description                        | Default value  |
|----------------------|------------------------------------|----------------|
| metadata.elasticsearch.hosts | hosts  | http://elasticsearch.doc-store:9200|
| metadata.elasticsearch.index_name | elasticsearch index  | artifacts-metadata|
| metadata.elasticsearch.credentials.user | elasticsearch user  | |
| metadata.elasticsearch.credentials.user | elasticsearch password  | |
| metadata.elasticsearch.credentials.token | elasticsearch token value  | |
| metadata.elasticsearch.credentials.token_type | elasticsearch token type  | |


!!! info

    Remember to put elasticsearch mapping first! Where you couldn't list the artifacts available.


##### Configmap metadata

You can use Configmap backend for metadata.

```yaml
configmap:
  apiUrl: <kubernetes API URL>
```

| Configuration        | Description                        | Default value  |
|----------------------|------------------------------------|----------------|
| metadata.configmap.apiUrl | Kubernetes API URL | |


##### Filesystem metadata

You can use Filesystem backend for metadata.

```yaml
metadata:
  file: /tmp/artifacts/metadata
```

| Configuration        | Description                        | Default value  |
|----------------------|------------------------------------|----------------|
| metadata.file        | Filepath where metadata are saved  | /tmp/artifacts/metadata|

#### Data

##### Minio data

You can use Minio (S3) backend for data.

```yaml
data:
  minio:
    host: http://s3.object-store:9000
    access_key: minio
    secret_key: K@st2020*
```

| Configuration        | Description                        | Default value  |
|----------------------|------------------------------------|----------------|
| data.minio.host      | hostname                           | http://s3.object-store:9000|
| data.minio.access_key | username / access key             | minio |
| data.minio.secret_key | password / secret key             | K@st2020*|

!!! info

    Remember to create the S3 bucket (corresponding to tenant.name) before! Where you couldn't upload artifacts.


##### Filesystem backend

You can use Filesystem backend for data.

```yaml
data:
  file: /tmp/artifacts/data
```

| Configuration        | Description                        | Default value  |
|----------------------|------------------------------------|----------------|
| data.file            | Filepath where data are saved      | /tmp/artifacts/data|


#### OAuth2

Artifact server can be secured with OAUTH, for example with **keycloak**.  

##### Setup OAUTH provider

Configuration bellow is default configuration used by SpringBoot using OAUTH.
You can configure others providers and registrations reading official SpringBoot documentation.

```yaml
# Add this configuration in application.yml
spring:
  security:
    enabled: true
    oauth2:
      client:
        provider:
          keycloak:
            authorization-uri: http://localhost:8080/auth/realms/ArtifactServerRealm/protocol/openid-connect/auth
            token-uri: http://localhost:8080/auth/realms/ArtifactServerRealm/protocol/openid-connect/token
            user-info-uri: http://localhost:8080/auth/realms/ArtifactServerRealm/protocol/openid-connect/userinfo
            jwk-set-uri: http://localhost:8080/auth/realms/ArtifactServerRealm/protocol/openid-connect/certs
            user-name-attribute: preferred_username
        registration:
          keycloak:
            client-id: login-app
            provider: keycloak
            scope: openid
            redirect-uri: http://localhost:4245/login/oauth2/code/keycloak
            client-authentication-method: basic
            authorization-grant-type: authorization_code
```

##### Bind roles with permissions

Keycloak can be used to provide **roles**. You can map provider roles (keycloak) to permissions and create full customizable application roles.


| permission | description |
|------------|--------------|
|UPLOAD_ARTIFACTS | Permission to allow upload artifacts |
|DELETE_ARTIFACTS | Permission to allow delete artifacts |


For example, if you use keycloak with **USER** and **ADMIN** roles you can configure artifact server like this:

```yaml
application:
  roles:
    ROLE_USER: # <= keycloak roles are prefixed by "ROLE_" in JWT 
      UPLOAD_ARTIFACTS
    ROLE_ADMIN: # <= keycloak roles are prefixed by "ROLE_" in JWT
      UPLOAD_ARTIFACTS
      DELETE_ARTIFACTS
```

