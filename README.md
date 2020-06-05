# Cloud Native Integration CI/CD Demo

## Prerequisites

* OpenShift v4.4 (Tested on [CodeReady Containers](https://code-ready.github.io/crc/))

## Demo Instructions

### Fork This Repository

This repository contains branches `ci` and `dev` that represent the desired 
state for those respective demo environments. ArgoCD will keep target namespaces
synchronized with resources committed to those branches. In order to test and 
make changes yourself, create your own fork of this repository.

### Install Global Operators from OperatorHub

```bash
oc apply -f operators/openshift-pipelines.yml -n openshift-operators
oc apply -f operators/openshift-serverless.yml -n openshift-operators
```

### Install ArgoCD Operator from OperatorHub

```bash
oc new-project argocd
oc apply -f operators/argocd.yml -n argocd
```

### Create ArgoCD Instance

```bash
oc apply -f argocd/argocd.yml -n argocd
oc apply -f argocd/route.yml -n argocd
```

### Login to ArgoCD

Get password for `admin` user:

```bash
oc get secret cni-demo-argocd-cluster -o jsonpath='{.data.admin\.password}' -n argocd | base64 -d
```

Get the URL to ArgoCD:

```bash
echo https://$(oc get routes argocd -o jsonpath='{ .spec.host }' -n argocd)
```

Use the URL and credentials determined in the previous commands to login to the
ArgoCD UI. 

From the UI, the documentation tab will provide a link to the `argocd`
CLI download. Download the CLI, and add it to your path, then login using:

```bash
argocd login $(oc get routes argocd -o jsonpath='{ .spec.host }' -n argocd):443
```
