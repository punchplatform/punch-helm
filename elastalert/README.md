# Punchplatform Elastalert

The Punchplatform Archive Housekeeping permits you to define a strategy to clean old data.

## Configuration

### Kubernetes configuration

| Configuration        | Description                         | Default value                                               |
|----------------------|-------------------------------------|-------------------------------------------------------------|
| image.name           | The ES Housekeeping docker image    | artifactory.thalesdigital.io/private-docker-punch/product/pp-punch/elastalert |
| image.policy         | The Kubernetes image pull policy    | IfNotPresent                                                |
| image.secret | Secret name required to pull docker image |                                                             |

#### Elasticsearch configuration

| Configuration              | Description                      | Default value      |
|----------------------------|----------------------------------|--------------------|
| elasticsearch.host     | Elasticsearch host   | elasticsearch.doc-store |
| elasticsearch.port   | Elasticsearch port | 9200 |
| elasticsearch.index   | Elastalert index name | elastalert |

#### Archive configuration

| Configuration              | Description                      | Default value      |
|----------------------------|----------------------------------|--------------------|
| pools    | List of housekeeping actions  |  Empty |
| schedule   | Call frequency | */1 * * * * |