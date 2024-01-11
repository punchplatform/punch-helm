# artifacts-server

![Version: 8.1-dev](https://img.shields.io/badge/Version-8.1--dev-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 8.1-dev](https://img.shields.io/badge/AppVersion-8.1--dev-informational?style=flat-square)

Helm chart for the Punch Artifacts Server

## Values

| Key                                         | Type   | Default                                                                                                           | Description                                                               |
|---------------------------------------------|--------|-------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| config.application.tenant                   | string | `"mytenant"`                                                                                                      | tenant to store data                                                      |
| config.data.file                            | string | `nil`                                                                                                             | path to folder for FS data backend                                        |
| config.data.minio                           | object | `{"access_key":"minio","host":"http://s3.object-store:9000","secret_key":"password", "skipCreateBucket":"false"}` | Connection information for Minio data backend and for bucket creation     |
| config.data.type                            | string | `minio`                                                                                                           | to explicitly specify the data backend to be used                         |
| config.keycloak.auth_server_url             | string | `"http://keycloak-http.authentication/auth/"`                                                                     | Keycloak auth server informations                                         |
| config.keycloak.principal_attribute         | string | `"preferred_username"`                                                                                            |                                                                           |
| config.keycloak.public_client               | bool   | `true`                                                                                                            |                                                                           |
| config.keycloak.realm                       | string | `"kast"`                                                                                                          |                                                                           |
| config.keycloak.resource                    | string | `"artifact-server"`                                                                                               |                                                                           |
| config.keycloak.use_resource_role_mappings  | bool   | `true`                                                                                                            |                                                                           |
| config.metadata.file                        | string | `nil`                                                                                                             | path to folder for FS metadata backend                                    |
| config.metadata.minio                       | object | `{"access_key":"minio","host":"http://s3.object-store:9000","secret_key":"password","skipCreateBucket":"false"}`  | Connection information for Minio metadata backend and for bucket creation |
| config.metadata.type                        | string | `minio`                                                                                                           | to explicitly specify the metadata backend to be used                     |
| config.security.enabled                     | bool   | `false`                                                                                                           | enable Artifacts Server security with keycloak                            |
| config.server.port                          | int    | `4245`                                                                                                            | port to run the app                                                       |
| config.spring                               | object | `{"servlet":{"multipart":{"max-file-size":"100MB","max-request-size":"100MB"}}}`                                  | spring configuration                                                      |
| config.web.enabled                          | bool   | `false`                                                                                                           | enable Artifacts Server UI                                                |
| image.name                                  | string | `"ghcr.io/punchplatform/artifacts-server:8.1-dev"`                                                                |                                                                           |
| image.policy                                | string | `"IfNotPresent"`                                                                                                  |                                                                           |
| image.secret                                | string | `"admin-secret"`                                                                                                  | secret to pull image                                                      |
| serviceAccountName                          | string | `"default"`                                                                                                       | service account name                                                      |
| ingress.enabled                             | bool   | `true`                                                                                                            | enable an ingress                                                         |
| ingress.ingressClassName                    | string | `"nginx"`                                                                                                         | ingress class name to select an ingress controller                        |
| ingress.url                                 | string | `"artifacts-server.dpsc"`                                                                                         | ingress url                                                               |
| server.port                                 | int    | `4245`                                                                                                            | port to run the app                                                       |


