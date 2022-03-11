# artifacts-server

![Version: 8.0-dev](https://img.shields.io/badge/Version-8.0--dev-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square)

Helm chart for the Punch Artifacts Server

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| application.tenant | string | `"mytenant"` | tenant to store data |
| data.file | string | `nil` | path to folder for FS data backend |
| data.minio | object | `{"access_key":"minio","host":"http://s3.object-store:9000","secret_key":"password"}` | Connection information for Minio data backend |
| image.name | string | `"ghcr.io/punchplatform/artifacts-server:8.0-dev"` |  |
| image.policy | string | `"IfNotPresent"` |  |
| image.secret | string | `"admin-secret"` | secret to pull image |
| ingress.enabled | bool | `true` | enable an ingress |
| ingress.url | string | `"artifacts-server.dpsc"` | ingress url |
| metadata.elasticsearch | object | `{"http_hosts":[{"host":"elasticsearch.doc-store","port":9200,"scheme":"http"}],"index":"artifacts-metadata","security":{"credentials":{"password":"admin","username":"admin"},"ssl":{"hostname_verification":false,"use_self_signed_certificate":false}}}` | ES backend - connection information |
| metadata.file | string | `nil` | path to folder for FS metadata backend |
| server.port | int | `4245` | port to run the app |
| spring | object | `{"servlet":{"multipart":{"max-file-size":"100MB","max-request-size":"100MB"}}}` | spring configuration |
| web.enabled | bool | `true` | enable Artifacts Server UI |

