# homelab-gitops
ArgoCD Syncing

## Overview
Some of the containers on my home lab will be managed by k3s. This is because I want to utilize my mac mini for compute, without having to treat it like a separate machine.

GitOps + ArgoCD automate my deployments to k3s. This is my repository for making changes to these deployments.

I will have a private dns server running on each of my nodes. A load balancer will ensure traffic reaches either.

## Kicking of the App of Apps
kubectl apply -f https://raw.githubusercontent.com/ConnorGoodman/homelab-gitops/main/root-app.yaml

## Setting up the fantasy exporter
There are two league types, Sleeper and ESPN. Sleeper is public, ESPN requires credentials.

To set up the ESPN creds, run: 

kubectl -n fantasy-exporter create secret generic fantasy-exporter-espn `
  --from-literal=ESPN_SWID='your-swid' `
  --from-literal=ESPN_S2='your-s2'