# Punch Operator

## Overview

The punch operator is actually a set of several Kubernetes Operators 
in charge of managing the punch CRDs (Custom Resources Definitions). 
Several types of CRDs are supported by the Punch: 

- Stormline: used to model a storm-compatible punchline. 
- Flinkline: used to model a flink-compatible punchline. 
- Sparkline: used to model a spark-compatible punchline.
- Application: used to model arbitrary applications.
- Plan: used to model a plan. A plan is a sort of CronJob extended with an exactly once guarantee for executing periodic punchlines. 

## Usage

Once the operator is up and running on your Kubernetes (see below) here is how to use it:

To start a punchline (storm flink or spark) siply type:

```sh
kubectl apply -f <punchline_crd.yaml>
```

To stop it:

```sh
kubectl delete -f <punchline_crd.yaml>
```

To list the deployed punchlines: 

```sh
kubectl get stormlines
kubectl get sparklines
kubectl get flinklines
kubectl get platforms
```

Starting a plan is similar. I.e you use the same commands. 

```sh
kubectl apply -f <plan_crd.yaml>
```

To stop it:

```sh
kubectl delete -f <plan_crd.yaml>
```

```sh
kubectl get plans
```

## Configuration

### Kubernetes configuration

#### Image

| Configuration        | Description                         | Default value                                               |
|----------------------|-------------------------------------|-------------------------------------------------------------|
| image.name           | The Punch Operator docker image     | ghcr.io/punchplatform/operator:v1.0.2 |
| image.policy         | The Kubernetes image pull policy    | IfNotPresent                                                |
| image.secret         | Secret name to pull docker image    | admin-secret                                                          |

### Punchplatform Operator configuration

#### Stormline

| Configuration     | Description                                                      | Default value |
|-------------------|------------------------------------------------------------------|---------------|
| stormline.enable  | If Stormline CRD is enabled                                      |   true        |
| stormline.workers | Number of threads to manage Stormline CRDs. This is not the number of workers within a topology. | 2  |

#### Flinkline

| Configuration     | Description                                                      | Default value |
|-------------------|------------------------------------------------------------------|---------------|
| flinkline.enable  | If Flinkline CRD is enabled                                      |   true        |
| flinkline.workers | Number of threads to manage Flinkline CRDs. This is not the number of workers within a topology. | 2  |

#### Sparkline

| Configuration     | Description                                                      | Default value |
|-------------------|------------------------------------------------------------------|---------------|
| sparkline.enable  | If Sparkline CRD is enabled                                      |   true        |
| sparkline.workers | Number of threads to manage Sparkline CRDs. This is not the number of workers within a topology. | 2  |

#### Application

| Configuration     | Description                                                      | Default value |
|-------------------|------------------------------------------------------------------|---------------|
| application.enable  | If Application CRD is enabled                                      |   true        |
| application.workers | Number of threads to manage Application CRDs. This is not the number of workers within the application. | 2  |

#### Plan

| Configuration     | Description                                                      | Default value |
|-------------------|------------------------------------------------------------------|---------------|
| plan.enable  | If Plan CRD is enabled                                      |   true        |
| plan.workers | Number of threads to manage Plan CRDs. This is not the number of workers within the planned application. | 2  |

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
