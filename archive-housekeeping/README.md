# Punchplatform Archive Housekeeping

The Punchplatform Archive Housekeeping permits you to define a strategy to clean old data.

## Configuration

### Kubernetes configuration

| Configuration        | Description                         | Default value                                               |
|----------------------|-------------------------------------|-------------------------------------------------------------|
| image.name           | The ES Housekeeping docker image    | ghcr.io/punchplatform/es-housekeeping |
| image.policy         | The Kubernetes image pull policy    | IfNotPresent                                                |
| image.secret | Secret name required to pull docker image |                                                             |

#### Archive configuration

| Configuration              | Description                      | Default value      |
|----------------------------|----------------------------------|--------------------|
| pools    | List of housekeeping actions  |  Empty |
| schedule   | Call frequency | */1 * * * * |