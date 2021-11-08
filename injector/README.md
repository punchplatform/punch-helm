# Punch Injector

This chart constructs Kubernetes objects for a Punch log injector.

## Content

- Secret (to retrieve Punch image)
- Deployment with an init container to retrieve heavy resources from s3
- ConfigMaps for light resources (punchline, punchlets)

## Dependencies

When installing this chart, you must provide platform & channel variables containing s3 credentials, registry,
ingress...

It contains a dependency chart called channel which is composed with :

- Ingress
- Service
- Secret with S3 credentials
- Job to create bucket for tenant
- Create kafka topic

## Variables

```yaml
#(replica for app)
replicaCount: 1

#(id MUST BE the same as the --set-file arg dest in container)
configMap:
  - id: punchline
    dest: /opt/punch/conf/punchline.json
  - id: punchlet
    dest: /opt/punchlets/prepare_track_elastic.punch
  - id: location
    dest: /opt/punchlets/prepare_location_elastic.punch
  - id: vega
    dest: /opt/punchlets/prepare_vega_elastic.punch

#(id is free name is the name in s3 bucket dest path in container)
resources:
  - id: resource1
    name: gebd_storm_components-1.0-SNAPSHOT-jar-with-dependencies.jar
    path: /opt/punch/lib/gebd_storm_components-1.0-SNAPSHOT-jar-with-dependencies.jar
  - id: resource2
    name: pipeline_gebd_benchmarks_elasticsearch_punchlets_sensorMap.json
    path: /opt/resources/sensorMap.json
  - id: resource3
    name: samples_prt_Station_10.prt
    path: /opt/samples_prt/samples_prt_Station_10.prt
  - id: resource4
    name: samples_prt_Station_1.prt
    path: /opt/samples_prt/samples_prt_Station_1.prt
```

Additional platform & channel files are mandatory.

## Order

In this chart, Kube object are sorted as follows :

- Create topic (weight -10)
- Check topic (weight -5)
- Create S3 secret (weight -3)
- Import objects from s3 (weight -1)

The other objects follows the classic Kube order :

```
var InstallOrder KindSortOrder = []string{
"Namespace",
"NetworkPolicy",
"ResourceQuota",
"LimitRange",
"PodSecurityPolicy",
"PodDisruptionBudget",
"Secret",
"ConfigMap",
"StorageClass",
"PersistentVolume",
"PersistentVolumeClaim",
"ServiceAccount",
"CustomResourceDefinition",
"ClusterRole",
"ClusterRoleList",
"ClusterRoleBinding",
"ClusterRoleBindingList",
"Role",
"RoleList",
"RoleBinding",
"RoleBindingList",
"Service",
"DaemonSet",
"Pod",
"ReplicationController",
"ReplicaSet",
"Deployment",
"HorizontalPodAutoscaler",
"StatefulSet",
"Job",
"CronJob",
"Ingress",
"APIService", }
```

## Example

```sh
helm install punchline punchline \
    --set-file punchline=/home/punch/workspace/standalone/6.3/punch-standalone-6.3.3-SNAPSHOT-linux/punchline.json \
    --set-file punchlet=/home/punch/workspace/gebd/pipeline/gebd_benchmarks/elasticsearch/punchlets/prepare_track_elastic.punch \
    --set-file location=/home/punch/workspace/gebd/pipeline/gebd_benchmarks/elasticsearch/punchlets/prepare_location_elastic.punch \
    --set-file vega=/home/punch/workspace/gebd/pipeline/gebd_benchmarks/elasticsearch/punchlets/prepare_vega_elastic.punch \
    -f platform.yaml --namespace tenant -f channel/channel.yaml -f punchline/punchline.yaml
```

Files which must be passed as configMap must be provided using `--set-file` using the same id the declared one 