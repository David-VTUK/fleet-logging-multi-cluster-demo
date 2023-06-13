# Multi Cluster Logging Demo

This repo contains Fleet resources used to:

* Install the Logging stack into downstream clusters

```yaml
apiVersion: fleet.cattle.io/v1alpha1
kind: GitRepo
metadata:
  name: manifests
  namespace: fleet-default
spec:
  forceSyncGeneration: 2
  insecureSkipTLSVerify: false
  repo: https://github.com/David-VTUK/fleet-logging-multi-cluster-demo
  targets:
    - clusterSelector:
        matchLabels:
          env: emea
      name: emea
    - clusterSelector:
        matchLabels:
          env: apac
      name: apac
    - clusterSelector:
        matchLabels:
          env: americas
      name: americas
```

If any registered cluster has the cluster label `env = emea/apac/americas` then fleet will

1. Install the Logging chart CRD's
2. Install the Logging chart (which is dependent on 1.)
3. Install the Logging `clusterflow` and `clusteroutput` configs specific to that region


This is to demonstrate how dynamic configuration can be applied to observability stacks such as the Logging operator so managed clusters can be configured with different properties based on their labels.