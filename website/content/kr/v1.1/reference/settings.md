---
title: Settings
linkTitle: Settings
weight: 5
description: Configure Karpenter
---

# Settings

Karpenter surfaces environment variables and CLI parameters to allow you to configure certain global settings on the controllers. These settings are described below.

| Environment Variable          | CLI Flag                     | Description                                                                                                                                                                                                                                                                                                 |
| ----------------------------- | ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| BATCH\_IDLE\_DURATION         | --batch-idle-duration        | The maximum amount of time with no new pending pods that if exceeded ends the current batching window. If pods arrive faster than this time, the batching window will be extended up to the maxDuration. If they arrive slower, the pods will be batched separately. (default = 1s)                         |
| BATCH\_MAX\_DURATION          | --batch-max-duration         | The maximum length of a batch window. The longer this is, the more pods we can consider for provisioning at one time which usually results in fewer but larger nodes. (default = 10s)                                                                                                                       |
| CLUSTER\_CA\_BUNDLE           | --cluster-ca-bundle          | Cluster CA bundle for nodes to use for TLS connections with the API server. If not set, this is taken from the controller's TLS configuration.                                                                                                                                                              |
| CLUSTER\_ENDPOINT             | --cluster-endpoint           | The external kubernetes cluster endpoint for new nodes to connect with. If not specified, will discover the cluster endpoint using DescribeCluster API.                                                                                                                                                     |
| CLUSTER\_NAME                 | --cluster-name               | \[REQUIRED] The kubernetes cluster name for resource discovery.                                                                                                                                                                                                                                             |
| DISABLE\_LEADER\_ELECTION     | --disable-leader-election    | Disable the leader election client before executing the main loop. Disable when running replicated components for high availability is not desired.                                                                                                                                                         |
| EKS\_CONTROL\_PLANE           | --eks-control-plane          | Marking this true means that your cluster is running with an EKS control plane and Karpenter should attempt to discover cluster details from the DescribeCluster API                                                                                                                                        |
| ENABLE\_PROFILING             | --enable-profiling           | Enable the profiling on the metric endpoint                                                                                                                                                                                                                                                                 |
| FEATURE\_GATES                | --feature-gates              | Optional features can be enabled / disabled using feature gates. Current options are: SpotToSpotConsolidation (default = NodeRepair=false,SpotToSpotConsolidation=false)                                                                                                                                    |
| HEALTH\_PROBE\_PORT           | --health-probe-port          | The port the health probe endpoint binds to for reporting controller health (default = 8081)                                                                                                                                                                                                                |
| INTERRUPTION\_QUEUE           | --interruption-queue         | Interruption queue is the name of the SQS queue used for processing interruption events from EC2. Interruption handling is disabled if not specified. Enabling interruption handling may require additional permissions on the controller service account. Additional permissions are outlined in the docs. |
| ISOLATED\_VPC                 | --isolated-vpc               | If true, then assume we can't reach AWS services which don't have a VPC endpoint. This also has the effect of disabling look-ups to the AWS on-demand pricing endpoint.                                                                                                                                     |
| KARPENTER\_SERVICE            | --karpenter-service          | The Karpenter Service name for the dynamic webhook certificate                                                                                                                                                                                                                                              |
| KUBE\_CLIENT\_BURST           | --kube-client-burst          | The maximum allowed burst of queries to the kube-apiserver (default = 300)                                                                                                                                                                                                                                  |
| KUBE\_CLIENT\_QPS             | --kube-client-qps            | The smoothed rate of qps to kube-apiserver (default = 200)                                                                                                                                                                                                                                                  |
| LEADER\_ELECTION\_NAME        | --leader-election-name       | Leader election name to create and monitor the lease if running outside the cluster (default = karpenter-leader-election)                                                                                                                                                                                   |
| LEADER\_ELECTION\_NAMESPACE   | --leader-election-namespace  | Leader election namespace to create and monitor the lease if running outside the cluster                                                                                                                                                                                                                    |
| LOG\_ERROR\_OUTPUT\_PATHS     | --log-error-output-paths     | Optional comma separated paths for logging error output (default = stderr)                                                                                                                                                                                                                                  |
| LOG\_LEVEL                    | --log-level                  | Log verbosity level. Can be one of 'debug', 'info', or 'error' (default = info)                                                                                                                                                                                                                             |
| LOG\_OUTPUT\_PATHS            | --log-output-paths           | Optional comma separated paths for directing log output (default = stdout)                                                                                                                                                                                                                                  |
| MEMORY\_LIMIT                 | --memory-limit               | Memory limit on the container running the controller. The GC soft memory limit is set to 90% of this value. (default = -1)                                                                                                                                                                                  |
| METRICS\_PORT                 | --metrics-port               | The port the metric endpoint binds to for operating metrics about the controller itself (default = 8080)                                                                                                                                                                                                    |
| RESERVED\_ENIS                | --reserved-enis              | Reserved ENIs are not included in the calculations for max-pods or kube-reserved. This is most often used in the VPC CNI custom networking setup https://docs.aws.amazon.com/eks/latest/userguide/cni-custom-network.html. (default = 0)                                                                    |
| VM\_MEMORY\_OVERHEAD\_PERCENT | --vm-memory-overhead-percent | The VM memory overhead as a percent that will be subtracted from the total memory for all instance types when cached information is unavailable. (default = 0.075)                                                                                                                                          |

#### Feature Gates

Karpenter uses [feature gates](https://kubernetes.io/docs/reference/command-line-tools-reference/feature-gates/#feature-gates-for-alpha-or-beta-features) You can enable the feature gates through the `--feature-gates` CLI environment variable or the `FEATURE_GATES` environment variable in the Karpenter deployment. For example, you can configure drift, spotToSpotConsolidation by setting the CLI argument: `--feature-gates Drift=true,SpotToSpotConsolidation=true`.

| Feature                 | Default | Stage | Since   | Until   |
| ----------------------- | ------- | ----- | ------- | ------- |
| Drift                   | false   | Alpha | v0.21.x | v0.32.x |
| Drift                   | true    | Beta  | v0.33.x | v0.37.x |
| SpotToSpotConsolidation | false   | Alpha | v0.34.x |         |
| NodeRepair              | false   | Alpha | v1.1.x  |         |

{

} In v1, drift has been promoted to stable and the feature gate removed. Users can continue to control drift by using disruption budgets by reason. Example:

```yaml
apiVersion: karpenter.sh/v1
kind: NodePool
metadata:
  name: default
spec:
…
  disruption:
    budgets:
    - nodes: 10%
    # On Weekdays during business hours, don't do any deprovisioning regarding drift.
    - nodes: "0"
      schedule: "0 9 * * mon-fri"
      duration: 8h
      reasons:
      -	Drifted
    # during non-business hours do drift for up to 10% of nodes
    - nodes: "10%"
      reasons:
      -	Drifted
```

{

}

#### Batching Parameters

The batching parameters control how Karpenter batches an incoming stream of pending pods. Reducing these values may trade off a slightly faster time from pending pod to node launch, in exchange for launching smaller nodes. Increasing the values can do the inverse. Karpenter provides reasonable defaults for these values, but if you have specific knowledge about your workloads you can tweak these parameters to match the expected rate of incoming pods.

For a standard deployment scale-up, the pods arrive at the QPS setting of the `kube-controller-manager`, and the default values are typically fine. These settings are intended for use cases where other systems may create large numbers of pods over a period of many seconds or minutes and there is a desire to batch them together.

**Batch Idle Duration**

The batch idle duration duration is the period of time that a new pending pod extends the current batching window. This can be increased to handle scenarios where pods arrive slower than one second part, but it would be preferable if they were batched together onto a single larger node.

This value is expressed as a string value like `10s`, `1m` or `2h45m`. The valid time units are `ns`, `us` (or `µs`), `ms`, `s`, `m`, `h`.

**Batch Max Duration**

The batch max duration is the maximum period of time a batching window can be extended to. Increasing this value will allow the maximum batch window size to increase to collect more pending pods into a single batch at the expense of a longer delay from when the first pending pod was created.

This value is expressed as a string value like `10s`, `1m` or `2h45m`. The valid time units are `ns`, `us` (or `µs`), `ms`, `s`, `m`, `h`.