# homelab-gitops
ArgoCD Syncing

## Overview
Some of the containers on my home lab will be managed by k3s. This is because I want to utilize my mac mini for compute, without having to treat it like a separate machine.

GitOps + ArgoCD automate my deployments to k3s. This is my repository for making changes to these deployments.

## Kicking of the App of Apps
kubectl apply -f https://raw.githubusercontent.com/ConnorGoodman/homelab-gitops/main/root-app.yaml