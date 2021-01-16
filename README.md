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
1. Fork this repositiory to your personal account
1. Run the following command:
```
export GITHUB_TOKEN=<your-github-token>
flux bootstrap github --owner=<your-github-name> \
  --repository=flux \
  --branch=master \
  --path=./clusters/stg3
  --personal
  --token-auth
```
Command will create repo `github.com/<your-github-name>/flux`
and installs FluxCD components to cluster.

## Wait and watch what happens
Reconcilation process should start when FluxCD components are deployed
and git repository starts to be periodically scanned for changes.
First, the Kafka operator is deployed to operators namespace.
Then kafka-related custom resources will be installed (after kustomize
transformation valid for stg3 cluster) and Kafka cluster comes to life
and Kafka topics are registred.

## Clone your repo and play
Clone your forked repo and do some changes. Refer documentation for examples,
how the Flux CLI help you when creating new resources. Or you can modify existing ones,
it's up to you.

When you finish your changes, commit them to your git repo. Eventually, the Flux source
controller will notice the repo change and trigger requested actions.
