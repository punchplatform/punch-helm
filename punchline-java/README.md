# Goals

This chart constructs Kubernetes objects for a Punchline java. Resource management is not deal with this helm chart

# Content

- Create a deployment
- Create a configMap with a provided punchline

# Variables

A default values file is provided, check his content to see chart variables

# Example

With the default punchline :

helm install punchline-exemple helm/punchline-java

Or with your custom punchline :

helm install punchline-example helm/punchline-java --set-file punchline=src/test/resources/syslog_to_syslog.yaml

Punchline file must be passed using --set-file into the key "punchline"