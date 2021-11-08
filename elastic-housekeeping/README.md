# Punchplatform Elasticsearch Housekeeping

The Punchplatform Elasticsearch Housekeeping is in charge of cleaning up or moving data around as required.

## Configuration

### Kubernetes configuration

| Configuration        | Description                         | Default value                                               |
|----------------------|-------------------------------------|-------------------------------------------------------------|
| image.name           | The ES Housekeeping docker image    | artifactory.thalesdigital.io/private-docker-punch/product/pp-punch/es-housekeeping |
| image.policy         | The Kubernetes image pull policy    | IfNotPresent                                                |
| image.secret         | Secret name to pull docker image    |      admin-secret                                                       |

#### Elasticsearch configuration

| Configuration              | Description                      | Default value      |
|----------------------------|----------------------------------|--------------------|
| elasticsearch.host     | Elasticsearch host   | elasticsearch.doc-store |
| elasticsearch.port   | Elasticsearch port | 9200 |

#### Housekeeping configuration

| Configuration              | Description                      | Default value      |
|----------------------------|----------------------------------|--------------------|
| actions    | List of housekeeping actions  |  Empty |
| tenant   | Tenant in which housekeeping must be applied | mytenant |
| schedule   | Call frequency | */1 * * * * |