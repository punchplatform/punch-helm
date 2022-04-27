# Punch Operator

## Overview

The punch operator is actually a set of several Kubernetes Operators 
in charge of managing the punch CRDs (Custom Resources Definitions). 
Several types of CRDs are supported by the Punch: 

- Flinkline: used to model a flink-compatible punchline. 
- SparkPunchline: used to model a spark-compatible punchline.
- BatchApplication: used to model arbitrary batch applications.
- StreamApplication: used to model arbitrary streaming applications.
- BatchPunchline: used to model batch punchline.
- StreamPunchline: used to model streaming punchline.
- Plan: used to model a plan. A plan is a sort of CronJob extended with an exactly once guarantee for executing periodic punchlines. 

## Usage

Once the operator is up and running on your Kubernetes (see below) here is how to use it:

To start a punchline (flink,spark,bapp,sapp,bpun,spun) simply type:

```sh
kubectl apply -f <punchline_crd.yaml>
```

To stop it:

```sh
kubectl delete -f <punchline_crd.yaml>
```

To list the deployed punchlines: 

```sh
kubectl get sparkpunchlines
kubectl get flinklines
kubectl get platforms
kubectl get batchapplication
kubectl get streamapplication
kubectl get batchpunchline
kubectl get streampunchline
```

Starting a plan is similar. I.e you use the same commands. 

```sh
kubectl apply -f <plan_crd.yaml>
kubectl apply -f <diagram_crd.yaml>
```

To stop it:

```sh
kubectl delete -f <plan_crd.yaml>
kubectl delete -f <diagram_crd.yaml>
```

```sh
# schedulers
kubectl get plans
kubectl get diagrams
```

## Configuration

### Kubernetes configuration

#### Image

| Configuration              | Description                         | Default value                                               |
|----------------------------|-------------------------------------|-------------------------------------------------------------|
| kubernetes.image           | The Punch Operator docker image     | ghcr.io/punchplatform/operator:v1.0.2                       |
| kubernetes.imagePullPolicy | The Kubernetes image pull policy    | IfNotPresent                                                |
| kubernetes.imageSecret     | Secret name to pull docker image    | admin-secret                                                |

### Punchplatform Operator configuration

#### Annotations

| Configuration          | Description                                                      | Default value |
|------------------------|------------------------------------------------------------------|---------------|
| additionalAnnotations  | Add custom annotations for pod Operator                          |   empty       |
| additionalLabels       | Add custom labels for pod Operator                               |   empty       |


#### Flinkline

| Configuration                    | Description                                                                                      | Default value |
|----------------------------------|--------------------------------------------------------------------------------------------------|---------------|
| operator.punch.flinkline.enable  | If Flinkline CRD is enabled                                                                      |   true        |
| operator.punch.flinkline.workers | Number of threads to manage Flinkline CRDs. This is not the number of workers within a topology. |   2           |  

#### SparkPunchline

| Configuration                         | Description                                                                                      | Default value |
|---------------------------------------|--------------------------------------------------------------------------------------------------|---------------|
| operator.punch.sparkpunchline.enable  | If SparkPunchline CRD is enabled                                                                      |   true   |
| operator.punch.sparkpunchline.workers | Number of threads to manage SparkPunchline CRDs. This is not the number of workers within a topology. |   2      | 

#### BatchApplication

| Configuration                           | Description                                                                                             | Default value |
|-----------------------------------------|---------------------------------------------------------------------------------------------------------|---------------|
| operator.punch.batchApplication.enable  | If Application CRD is enabled                                                                           |   true        |
| operator.punch.batchApplication.workers | Number of threads to manage Application CRDs. This is not the number of workers within the application. |   2           |

#### StreamApplication

| Configuration                            | Description                                                                                             | Default value |
|------------------------------------------|---------------------------------------------------------------------------------------------------------|---------------|
| operator.punch.streamApplication.enable  | If Application CRD is enabled                                                                           |   true        |
| operator.punch.streamApplication.workers | Number of threads to manage Application CRDs. This is not the number of workers within the application. |   2           |

#### Plan

| Configuration                    | Description                                                                                              | Default value |
|----------------------------------|----------------------------------------------------------------------------------------------------------|---------------|
| operator.punch.plan.enable       | If Plan CRD is enabled                                                                                   |   true        |
| operator.punch.plan.workers      | Number of threads to manage Plan CRDs. This is not the number of workers within the planned application. |   2           |

#### Diagram

| Configuration                       | Description                                                                                                 | Default value |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------|---------------|
| operator.punch.diagram.enable       | If Diagram CRD is enabled                                                                                   |   true        |
| operator.punch.diagram.workers      | Number of threads to manage Diagram CRDs. This is not the number of workers within the planned application. |   2           |

## Install

The CRDs will be installed first, and then the punch Operator will be 
installed by Helm.

```sh
helm install my-punchoperator-release $PUNCHPLATFORM_HELM_CHARTS/operator -n punchoperator-system
```

## Uninstall

First, run :
```sh
helm uninstall my-punchoperator-release -n punchoperator-system
```

Helm will not remove CRD while uninstalling. This must be done manually.
Check [Helm limitations on CRD](https://helm.sh/docs/topics/charts/#limitations-on-crds).
