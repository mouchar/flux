# FluxCD demo

This is a sample demonstration of possibility to deploy HA Kafka
to K8S a GitOps way.

Techologies involved:
* FluxCD Toolkit
* kustomize
* Strimzi Kafka operator
* Helm
* Git
* and Kubernetes, obviously.

# Installation instructions

## Requirements
1. Git client
1. Kubernetes cluster admin access
1. Github or Gitlab token (with write access to your repo)

## Install the Flux CLI
Follow the steps in [documentation](https://toolkit.fluxcd.io/get-started/#install-the-flux-cli)

## Install Flux to cluster
Asuming you want to integrate with your Github account:
```
git clone https://github.com/mouchar/flux

export GITHUB_TOKEN=<your-github-token>
flux bootstrap github --owner=<your-github-name> \
  --repository=<your-repo-name> \
  --branch=master \
  --path=./clusters/stg3
  --personal
  --token-auth
```
Command will create repo github.com/<your-github-name>/<your-repo-name>
and installs FluxCD components to cluster.

## Wait and watch what happens
Reconcilation process should start when FluxCD components are deployed
and git repository starts to be periodically scanned for changes.
First, the Kafka operator is deployed to operators namespace.
Then kafka-related custom resources will be installed (after kustomize
transformation valid for stg3 cluster) and Kafka cluster comes to life
and Kafka topics are registred.


