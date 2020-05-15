# Canary Release using Istio

Canary release is a technique to reduce the risk of introducing a new software version in production by slowly rolling out the change to a small subset of users before rolling it out to the entire infrastructure and making it available to everybody.

![](./canary-release.png)

When you are happy with the new version, you can start routing a few selected users to it. As you gain more confidence in the new version, you can start releasing it to more servers in your infrastructure and routing more users to it.

For more details about canary release please refer [here](https://martinfowler.com/bliki/CanaryRelease.html)

### **PRE-REQUISITE**: Istio should already be installed in the same cluster.

## Installing the tasks

1. For Application Deployment

```
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/v1beta1/canary-deploy/canary-app-deploy.yaml
```

2. For Istio Services

```
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/v1beta1/canary-deploy/canary-istio-deploy.yaml
```

## Installing the ClusterRoleBinding

```
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/v1beta1/canary-deploy/clusterrolebinding.yaml
```

## Workspaces

- **deployment-manifest-dir**: The workspace in which `ConfigMap` containing all the deployment manifests will be mounted.
- **istio-manifest-dir**: The workspace in which `ConfigMap` containing the istio related manifests will be mounted.

## Params for Canary-Istio-Deploy

- **COMMAND**: The kubectl command to execute (eg `apply`, `replace`. _default_: `apply`)

# Usage

1. Create `ConfigMap`

```
kubectl create configmap istio --from-file="gateway.yaml" --from-file="destinationrule.yaml" --from-file="virtualservice.yaml"
```

```
kubectl create configmap deployment --from-file="deployment.yaml" --from-file="service.yaml"
```

In case of app deployment and configuring Istio follow [this](./examples/run.yaml) example.

In case of just re-configuring the Istio follow [this](./examples/taskrun.yaml) example.
