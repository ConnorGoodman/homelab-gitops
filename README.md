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

To create the ESPN credentials Secret on the cluster, run this on Linux:

kubectl -n fantasy-exporter create secret generic fantasy-exporter-espn \
  --from-literal=ESPN_SWID='your-swid' \
  --from-literal=ESPN_S2='your-s2'

## Running the fantasy exporter manually

The exporter is normally run by its CronJob every six hours. To run it immediately,
create a one-time Job from the deployed CronJob:

```bash
kubectl -n fantasy-exporter create job fantasy-exporter-manual-$(date +%Y%m%d%H%M%S) --from=cronjob/fantasy-exporter-cronjob
```

On PowerShell, use:

```powershell
kubectl -n fantasy-exporter create job fantasy-exporter-manual-$(Get-Date -Format yyyyMMddHHmmss) --from=cronjob/fantasy-exporter-cronjob
```

Check the Job and its Pod:

```powershell
kubectl -n fantasy-exporter get jobs,pods
kubectl -n fantasy-exporter logs -l job-name=<job-name>
```

Delete the completed Job after reviewing it:

```powershell
kubectl -n fantasy-exporter delete job <job-name>
```

```ubuntu
sudo kubect-n fantasy-exporter create job fantasy-exporter-manual-$(date +%Y%m%d%H%M%S) --from=cronjob/fantasy-exporter-cronjob
```

The first manually created Job also acts as the first PVC consumer, so it allows
the `local-path` storage provisioner to bind `fantasy-exporter-pvc`.