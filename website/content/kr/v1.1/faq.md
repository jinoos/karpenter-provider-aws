---
title: FAQs
linkTitle: FAQs
weight: 60
description: Karpenter 자주 묻는 질문 리뷰
---

# FAQs

### 일반

#### Karpenter를 프로덕션에서 사용해도 안전한가요?

Karpenter v1은 첫 번째 안정 Karpenter API입니다. 향후 호환되지 않는 API 변경사항은 v2 버전이 필요합니다.

#### NodePool은 어떻게 특정 노드를 관리하기로 결정하나요?

Karpenter가 노드를 구성하고 관리하는 방법에 대한 정보는 \[NodePool 구성]\(\{{< ref "./concepts/#configuring-nodepools" >\}})을 참조하세요.

#### 어떤 클라우드 제공자가 지원되나요?

AWS가 Karpenter가 지원하는 첫 번째 클라우드 제공자이지만, 다른 클라우드 제공자와도 함께 사용할 수 있도록 설계되었습니다.

#### Karpenter용 자체 클라우드 제공자를 작성할 수 있나요?

네, 하지만 아직 문서화되어 있지 않습니다. Karpenter의 GitHub [cloudprovider](https://github.com/aws/karpenter-core/tree/v1.1.1/pkg/cloudprovider) 문서에서 AWS 제공자가 어떻게 구축되었는지 확인할 수 있지만, 코드의 다른 부분도 변경이 필요합니다.

#### Karpenter는 어떤 운영체제 노드를 배포하나요?

Karpenter는 \[EC2NodeClass의 AMI Family]\(\{{< ref "./concepts/nodeclasses#specamifamily" >\}})에 정의된 OS를 사용합니다.

#### 사용자 지정 운영체제 이미지를 제공할 수 있나요?

Karpenter는 노드의 \[운영체제]\(\{{< ref "./concepts/nodeclasses/#specamiselectorterms" >\}})를 구성하기 위한 여러 메커니즘을 제공합니다.

#### Karpenter가 혼합 아키텍처 클러스터(arm vs. amd)의 워크로드를 처리할 수 있나요?

Karpenter는 \[잘 알려진 레이블]\(\{{< ref "./concepts/scheduling/#supported-labels">\}})을 사용하여 다중 아키텍처 구성에 유연하게 대응합니다.

#### 어떤 RBAC 접근 권한이 필요한가요?

필요한 모든 RBAC 규칙은 Helm 차트 템플릿에서 찾을 수 있습니다. 자세한 내용은 [clusterrole-core.yaml](https://github.com/aws/karpenter/blob/v1.1.1/charts/karpenter/templates/clusterrole-core.yaml), [clusterrole.yaml](https://github.com/aws/karpenter/blob/v1.1.1/charts/karpenter/templates/clusterrole.yaml), [rolebinding.yaml](https://github.com/aws/karpenter/blob/v1.1.1/charts/karpenter/templates/rolebinding.yaml), [role.yaml](https://github.com/aws/karpenter/blob/v1.1.1/charts/karpenter/templates/role.yaml) 파일을 참조하세요.

#### Kubernetes 클러스터 외부에서 Karpenter를 실행할 수 있나요?

네, 컨트롤러가 Kubernetes API와 제공자 API에 대한 네트워크 및 IAM/RBAC 접근 권한이 있다면 가능합니다.

#### Karpenter의 보안 문제를 발견하면 어떻게 해야 하나요?

Karpenter 보안 문제를 보고하는 방법은 [보안 문제 보고](https://github.com/aws/karpenter/security/policy)를 참조하세요. 공개 GitHub 이슈를 생성하지 마세요.

### 호환성

#### Karpenter는 어떤 버전의 Kubernetes를 지원하나요?

출시된 Karpenter 버전별 지원되는 Kubernetes 버전을 확인하려면 \[업그레이드 섹션의 호환성 매트릭스]\(\{{< ref "./upgrading/compatibility#compatibility-matrix" >\}})를 참조하세요.

#### 어떤 Kubernetes 배포판이 지원되나요?

Karpenter는 최신 AWS Elastic Kubernetes Service(EKS)의 신규 또는 기존 설치와의 통합을 문서화하고 있습니다. 다른 Kubernetes 배포판(KOPs 등)도 사용할 수 있지만, 이러한 배포판에 대한 클라우드 제공자 권한 설정은 문서화되어 있지 않습니다.

#### Karpenter는 AWS 노드 그룹 기능과 어떻게 상호작용하나요?

NodePool은 EKS 관리형 노드 그룹 및 EC2 Auto Scaling 그룹과 같은 정적 용량 관리 솔루션과 함께 작동하도록 설계되었습니다. NodePool을 사용하여 모든 용량을 관리하거나, 동적 및 정적으로 관리되는 용량을 혼합한 모델을 사용하거나, 완전히 정적인 접근 방식을 사용할 수 있습니다. 대부분의 사용자는 단기적으로는 혼합 접근 방식을, 장기적으로는 NodePool 관리 방식을 사용할 것으로 예상됩니다.

#### Karpenter는 Kubernetes 기능과 어떻게 상호작용하나요?

* Kubernetes Cluster Autoscaler: Karpenter는 Cluster Autoscaler와 함께 작동할 수 있습니다. 자세한 내용은 \[Kubernetes Cluster Autoscaler]\(\{{< ref "./concepts/#kubernetes-cluster-autoscaler" >\}})를 참조하세요.
* Kubernetes Scheduler: Karpenter는 Kubernetes 스케줄러가 스케줄 불가능으로 표시한 파드의 스케줄링에 중점을 둡니다. Karpenter가 Kubernetes 스케줄러와 어떻게 상호작용하는지에 대한 자세한 내용은 \[스케줄링]\(\{{< ref "./concepts/scheduling" >\}})을 참조하세요.

### 프로비저닝

#### Karpenter NodePool은 어떤 기능을 지원하나요?

NodePool 예시와 기능 설명은 \[NodePool API 문서]\(\{{< ref "./concepts/nodepools" >\}})를 참조하세요.

#### 클러스터에서 여러 개의 (팀 기반) NodePool을 생성할 수 있나요?

네, NodePool은 레이블을 기반으로 여러 팀을 식별할 수 있습니다. 자세한 내용은 \[NodePool API 문서]\(\{{< ref "./concepts/nodepools" >\}})를 참조하세요.

#### 여러 NodePool이 정의되어 있을 때, 내 파드는 어떤 것을 사용하나요?

대기 중인 파드는 해당 파드의 요구사항과 일치하는 모든 NodePool에 의해 처리됩니다. 여러 NodePool이 파드 요구사항과 일치할 경우 순서는 보장되지 않습니다. NodePool이 상호 배타적으로 설정되는 것을 권장합니다. 특정 NodePool을 선택하려면 노드 선택기 `karpenter.sh/nodepool: my-nodepool`을 사용하세요.

#### 특정 네임스페이스에 대해서만 파드를 프로비저닝하도록 Karpenter를 구성할 수 있나요?

네임스페이스 기반 프로비저닝에 대한 기본 지원은 없습니다. Karpenter는 테인트/톨러레이션과 노드 선택기의 조합을 기반으로 파드의 하위 집합을 프로비저닝하도록 구성할 수 있습니다. 이를 통해 Karpenter는 `kube-scheduler`와 함께 작동하여 파드가 기존 노드에 스케줄될 수 있는지 결정하는 동일한 메커니즘을 사용하여 새 노드를 프로비저닝합니다. 이는 Karpenter가 자체적으로 바인딩하지 않았을 노드에 파드가 바인딩되는 시나리오를 방지합니다. 만약 이런 일이 발생한다면, 파드가 스케줄 불가능했다면 노드가 프로비저닝되지 않았을 텐데도 노드가 비어있지 않은 상태로 유지되고 수명이 연장될 수 있습니다.

네임스페이스 기반 스케줄링 분리를 위해서는 Kubernetes 네이티브 스케줄링 제약 조건을 사용하는 것을 권장합니다. 네이티브 스케줄링 제약 조건을 사용하면 Karpenter, `kube-scheduler` 및 다른 모든 스케줄링 또는 자동 프로비저닝 메커니즘이 어떤 파드가 어떤 노드에 스케줄될 수 있는지에 대해 동일한 이해를 가질 수 있습니다. 이는 정책 에이전트를 통해 적용할 수 있으며, 예시는 [여기](https://blog.mikesir87.io/2022/01/creating-tenant-node-pools-with-karpenter/)에서 확인할 수 있습니다.

#### NodePool에 SSH 키를 추가할 수 있나요?

Karpenter는 NodePool이나 시크릿을 통해 관리하는 노드에 SSH 키를 추가하는 방법을 제공하지 않습니다. 하지만 Session Manager(SSM) 또는 EC2 Instance Connect를 사용하여 Karpenter 노드에 대한 셸 액세스를 얻을 수 있습니다. 명령줄에서 SSM 세션을 시작하는 예시는 \[Node NotReady]\(\{{< ref "./troubleshooting/#node-notready" >\}}) 문제 해결을 참조하거나, SSH를 사용하여 노드에 연결하는 방법은 [EC2 Instance Connect](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-connect-set-up.html) 문서를 참조하세요.

권장하지는 않지만, AWS 자격 증명 없이 Karpenter가 관리하는 노드에 액세스해야 하는 경우 EC2NodeClass User Data를 사용하여 SSH 키를 추가할 수 있습니다. 자세한 내용은 \[EC2NodeClass 문서의 User Data 섹션]\(\{{< ref "./concepts/nodeclasses/#specuserdata" >\}})을 참조하세요.

#### NodePool에 CPU와 메모리 제한을 설정할 수 있나요?

네. NodePool 예시와 제한 설정 방법에 대한 설명은 \[NodePool API 문서]\(\{{< ref "./concepts/nodepools#speclimits" >\}})를 참조하세요.

#### 스팟과 온디맨드 EC2 실행 유형을 혼합할 수 있나요?

네, 예시는 \[NodePool API 문서]\(\{{< ref "./concepts/nodepools#examples" >\}})를 참조하세요.

#### EC2 인스턴스 유형을 제한할 수 있나요?

* 속성 기반 요청은 현재 불가능합니다.
* GPU와 같은 특수 하드웨어가 있는 인스턴스를 선택할 수 있습니다.

#### 베어메탈 인스턴스 유형을 사용할 수 있나요?

네, NodePool의 `node.kubernetes.io/instance-type` 요구사항이 `metal` 인스턴스 유형만 포함할 때 Karpenter는 메탈 인스턴스 유형을 프로비저닝할 수 있습니다. 다른 인스턴스 유형이 파드 요구사항을 충족하는 경우, Karpenter는 메탈 인스턴스가 프로비저닝되기 전에 모든 비메탈 인스턴스 유형을 우선시합니다.

#### Karpenter는 어떻게 동적으로 인스턴스 유형을 선택하나요?

Karpenter는 대기 중인 파드를 배치하고 CPU, 메모리, GPU 요구사항에 따라 이들을 빈팩(binpack)합니다. 이때 노드 오버헤드, 필요한 VPC CNI 리소스, 새 노드를 시작할 때 패킹될 데몬셋(daemonset)을 고려합니다. Karpenter는 대부분의 일반적인 워크로드에 대해 \[3세대 이상의 C, M, R 인스턴스 유형 사용을 권장]\(\{{< ref "./concepts/nodepools#spectemplatespecrequirements" >\}})하지만, 요구사항 섹션의 [instance-type](https://kubernetes.io/docs/reference/labels-annotations-taints/#nodekubernetesioinstance-type)잘 알려진 레이블로 NodePool 스펙에서 제한할 수 있습니다.

파드가 가장 효율적인 인스턴스 유형(즉, 파드 배치를 수용할 수 있는 가장 작은 인스턴스 유형)에 빈팩된 후, Karpenter는 가장 효율적인 패킹보다 큰 59개의 다른 인스턴스 유형을 선택하고, 모든 60개의 인스턴스 유형 옵션을 Amazon EC2 Fleet이라는 API에 전달합니다.

EC2 Fleet API는 [Price Capacity Optimized 할당 전략](https://aws.amazon.com/blogs/compute/introducing-price-capacity-optimized-allocation-strategy-for-ec2-spot-instances/)을 기반으로 인스턴스 유형을 프로비저닝하려고 시도합니다. 온디맨드 용량 유형의 경우 이는 실질적으로 `lowest-price` 할당 전략과 동일합니다. 스팟 용량 유형의 경우, Fleet은 가장 낮은 가격과 중단될 가능성이 가장 낮은 인스턴스 유형을 결정합니다. 이는 스팟에 대해 절대적으로 가장 낮은 가격의 인스턴스 유형을 제공하지 않을 수 있습니다.

#### Karpenter는 스케줄링을 시뮬레이션할 때 데몬셋의 리소스 사용량을 어떻게 계산하나요?

Karpenter는 현재 레이블 선택기/테인트 등을 사용하여 NodePool 수준에서 적용 가능한 데몬셋을 계산합니다. NodePool이 시작할 수 있거나 없는 특정 인스턴스에서 실행되는 것을 제외하는 데몬셋의 요구사항은 확인하지 않습니다. 현재로서는 테인트/톨러레이션 또는 레이블 선택기가 있는 여러 NodePool을 사용하여 데몬셋을 특정 NodePool에서 시작된 노드로만 제한하는 것이 권장됩니다.

#### 스팟 용량이 없는 경우는 어떻게 되나요?

스팟 용량 부족에 대한 가장 좋은 방어책은 Karpenter가 가능한 한 많은 서로 다른 인스턴스 유형을 프로비저닝하도록 허용하는 것입니다. 필요한 것보다 더 높은 사양(예: vCPU, 메모리 등)을 가진 인스턴스 유형이라도 온디맨드 인스턴스를 사용하는 것보다 스팟 마켓에서 더 저렴할 수 있습니다. 스팟 용량이 제한될 때 온디맨드 용량도 제한될 수 있는데, 이는 스팟이 근본적으로 여분의 온디맨드 용량이기 때문입니다.

Karpenter가 다양한 인스턴스 유형 세트에서 노드를 프로비저닝하도록 허용하면 스팟의 할인된 가격으로 인해 더 오래 스팟을 유지하고 비용을 낮출 수 있습니다. 더욱이 스팟 용량이 제한되는 경우, 이러한 인스턴스 유형 다양성은 워크로드에 대한 온디맨드 용량을 계속 시작할 수 있는 가능성도 높여줍니다.

Karpenter는 각 인스턴스 유형에 대해 존과 용량 유형의 조합인 "오퍼링" 개념을 가지고 있습니다. Fleet API가 스팟 인스턴스에 대해 용량 부족 오류를 반환할 때마다, 해당 특정 오퍼링은 Karpenter가 다른 옵션으로 진행할 수 있도록 (전체 NodePool에서) 일시적으로 고려 대상에서 제외됩니다.

#### Does Karpenter support IPv6?

Yes! Karpenter dynamically discovers if you are running in an IPv6 cluster by checking the kube-dns service's cluster-ip. When using an AMI Family such as `AL2`, Karpenter will automatically configure the EKS Bootstrap script for IPv6. Some EC2 instance types do not support IPv6 and the Amazon VPC CNI only supports instance types that run on the Nitro hypervisor. It's best to add a requirement to your NodePool to only allow Nitro instance types:

```
apiVersion: karpenter.sh/v1
kind: NodePool
...
spec:
  template:
    spec:
      requirements:
        - key: karpenter.k8s.aws/instance-hypervisor
          operator: In
          values:
            - nitro
```

For more documentation on enabling IPv6 with the Amazon VPC CNI, see the [docs](https://docs.aws.amazon.com/eks/latest/userguide/cni-ipv6.html).

{

} Windows nodes do not support IPv6. {}

#### Why do I see extra nodes get launched to schedule pending pods that remain empty and are later removed?

You might have a daemonset, userData configuration, or some other workload that applies a taint after a node is provisioned. After the taint is applied, Karpenter will detect that the pod cannot be scheduled to this new node due to the added taint. As a result, Karpenter will provision yet another node. Typically, the original node has the taint removed and the pod schedules to it, leaving the extra new node unused and reaped by emptiness/consolidation. If the taint is not removed quickly enough, Karpenter may remove the original node before the pod can be scheduled via emptiness consolidation. This could result in an infinite loop of nodes being provisioned and consolidated without the pending pod ever scheduling.

The solution is to configure \[startupTaints]\(\{{\<ref "./concepts/nodepools/#cilium-startup-taint" >\}}) to make Karpenter aware of any temporary taints that are needed to ensure that pods do not schedule on nodes that are not yet ready to receive them.

Here's an example for Cilium's startup taint.

```
apiVersion: karpenter.sh/v1
kind: NodePool
...
spec:
  template:
    spec:
      startupTaints:
        - key: node.cilium.io/agent-not-ready
          effect: NoSchedule
```

### Scheduling

#### When using preferred scheduling constraints, Karpenter launches the correct number of nodes at first. Why do they then sometimes get consolidated immediately?

`kube-scheduler` is responsible for the scheduling of pods, while Karpenter launches the capacity. When using any sort of preferred scheduling constraint, `kube-scheduler` will schedule pods to nodes anytime it is possible.

As an example, suppose you scale up a deployment with a preferred zonal topology spread and none of the newly created pods can run on your existing cluster. Karpenter will then launch multiple nodes to satisfy that preference. If a) one of the nodes becomes ready slightly faster than other nodes and b) has enough capacity for multiple pods, `kube-scheduler` will schedule as many pods as possible to the single ready node, so they won't remain unschedulable. It doesn't consider the in-flight capacity that will be ready in a few seconds. If all the pods fit on the single node, the remaining nodes that Karpenter has launched aren't needed when they become ready and consolidation will delete them.

#### When deploying an additional DaemonSet to my cluster, why does Karpenter not scale-up my nodes to support the extra DaemonSet?

Karpenter will not scale-up more capacity for an additional DaemonSet on its own. This is due to the fact that the only pod that would schedule to that new node would be the DaemonSet pod, which is consuming additional capacity with no benefit. Therefore, Karpenter only considers DaemonSets when doing overhead calculations for scale-ups to workload pods.

To avoid new DaemonSets failing to schedule to existing Nodes, you should [set a high priority on your DaemonSet pods with a `preemptionPolicy: PreemptLowerPriority`](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/#example-priorityclass) so that DaemonSet pods will be guaranteed to schedule on all existing and new Nodes. For existing Nodes, this will cause some pods with lower priority to get [preempted](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/#preemption), replaced by the DaemonSet and re-scheduled onto new capacity that Karpenter will launch in response to the new pending pods.

The Karpenter maintainer team is also discussing a consolidation mechanism [in this Github issue](https://github.com/aws/karpenter/issues/3256) that would allow existing capacity to be rolled when a new DaemonSet is deployed without having to set [priority or preemption](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/) on the pods.

#### Why aren’t my Topology Spread Constraints spreading pods across zones?

Karpenter will provision nodes according to `topologySpreadConstraints`. However, the Kubernetes scheduler may schedule pods to nodes that do not fulfill zonal spread constraints if the `minDomains` field is not set. If Karpenter launches nodes that can handle more than the required number of pods, and the newly launched nodes initialize at different times, then the Kubernetes scheduler may place more than the desired number of pods on the first node that is Ready.

The preferred solution is to use the [`minDomains` field in `topologySpreadConstraints`](https://kubernetes.io/docs/concepts/scheduling-eviction/topology-spread-constraints/#topologyspreadconstraints-field), which is enabled by default starting in Kubernetes 1.27.

Before `minDomains` was available, another workaround has been to launch a lower [Priority](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/) pause container to each zone before launching the pods that you want to spread across the zones. The lower Priority on these pause pods would mean that they would be preempted when your desired pods are scheduled.

### Workloads

#### How can someone deploying pods take advantage of Karpenter?

See \[Application developer]\(\{{< ref "./concepts/#application-developer" >\}}) for descriptions of how Karpenter matches nodes with pod requests.

#### Can I use Karpenter with EBS disks per availability zone?

Yes. See \[Persistent Volume Topology]\(\{{< ref "./concepts/scheduling#persistent-volume-topology" >\}}) for details.

#### Can I set `--max-pods` on my nodes?

Yes, see the \[KubeletConfiguration Section in the NodePool docs]\(\{{\<ref "./concepts/nodepools#spectemplatespeckubelet" >\}}) to learn more.

#### Why do the Windows2019 and Windows2022 AMI families only support Windows Server Core?

The difference between the Core and Full variants is that Core is a minimal OS with less components and no graphic user interface (GUI) or desktop experience. `Windows2019` and `Windows2022` AMI families use the Windows Server Core option for simplicity, but if required, you can specify a custom AMI to run Windows Server Full.

You can specify the [Amazon EKS optimized AMI](https://docs.aws.amazon.com/eks/latest/userguide/eks-optimized-windows-ami.html) with Windows Server 2022 Full for Kubernetes 1.31 by configuring an `amiSelector` that references the AMI name.

```yaml
amiSelectorTerms:
    - name: Windows_Server-2022-English-Full-EKS_Optimized-1.31*
```

#### Can I use Karpenter to scale my workload's pods?

Karpenter is a node autoscaler which will create new nodes in response to unschedulable pods. Scaling the pods themselves is outside of its scope. This is the realm of pod autoscalers such as the [Vertical Pod Autoscaler](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler) (for scaling an individual pod's resources) or the [Horizontal Pod Autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) (for scaling replicas). We also recommend taking a look at [Keda](https://keda.sh/) if you're looking for more advanced autoscaling capabilities for pods.

### Deprovisioning

#### How does Karpenter deprovision nodes?

See \[Deprovisioning nodes]\(\{{< ref "./concepts/disruption" >\}}) for information on how Karpenter deprovisions nodes.

### Upgrading Karpenter

#### How do I upgrade Karpenter?

Karpenter is a controller that runs in your cluster, but it is not tied to a specific Kubernetes version, as the Cluster Autoscaler is. Use your existing upgrade mechanisms to upgrade your core add-ons in Kubernetes and keep Karpenter up to date on bug fixes and new features.

Karpenter requires proper permissions in the `KarpenterNode IAM Role` and the `KarpenterController IAM Role`. To upgrade Karpenter to version `$VERSION`, make sure that the `KarpenterNode IAM Role` and the `KarpenterController IAM Role` have the right permission described in `https://karpenter.sh/$VERSION/getting-started/getting-started-with-karpenter/cloudformation.yaml`. Next, locate `KarpenterController IAM Role` ARN (i.e., ARN of the resource created in [Create the KarpenterController IAM Role](../getting-started/getting-started-with-karpenter/#create-the-karpentercontroller-iam-role)) and pass them to the Helm upgrade command. {

}

For information on upgrading Karpenter, see the \[Upgrade Guide]\(\{{< ref "./upgrading/upgrade-guide/" >\}}).

### Upgrading Kubernetes Cluster

#### How do I upgrade an EKS Cluster with Karpenter?

{

} Karpenter recommends that you always validate AMIs in your lower environments before using them in production environments. Read \[Managing AMIs]\(\{{\<ref "./tasks/managing-amis" >\}}) to understand best practices about upgrading your AMIs.

If using a custom AMI, you will need to trigger the rollout of new worker node images through the publication of a new AMI with tags matching the \[`amiSelector`]\(\{{\<ref "./concepts/nodeclasses#specamiselectorterms" >\}}), or a change to the \[`amiSelector`]\(\{{\<ref "./concepts/nodeclasses#specamiselectorterms" >\}}) field. {

}

Karpenter's default behavior will upgrade your nodes when you've upgraded your Amazon EKS Cluster. Karpenter will \[drift]\(\{{\<ref "./concepts/disruption#drift" >\}}) nodes to stay in-sync with the EKS control plane version. Drift is enabled by default starting in `v0.33`. This means that as soon as your cluster is upgraded, Karpenter will auto-discover the new AMIs for that version.

Start by [upgrading the EKS Cluster control plane](https://docs.aws.amazon.com/eks/latest/userguide/update-cluster.html). After the EKS Cluster upgrade completes, Karpenter will Drift and disrupt the Karpenter-provisioned nodes using EKS Optimized AMIs for the previous cluster version by first spinning up replacement nodes. Karpenter respects [Pod Disruption Budgets](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) (PDB), and automatically \[replaces, cordons, and drains those nodes]\(\{{\<ref "./concepts/disruption#control-flow" >\}}). To best support pods moving to new nodes, follow Kubernetes best practices by setting appropriate pod [Resource Quotas](https://kubernetes.io/docs/concepts/policy/resource-quotas/) and using PDBs.

### Interruption Handling

#### Should I use Karpenter interruption handling alongside Node Termination Handler?

No. We recommend against using Node Termination Handler alongside Karpenter due to conflicts that could occur from the two components handling the same events.

#### Why should I migrate from Node Termination Handler?

Karpenter's native interruption handling offers two main benefits over the standalone Node Termination Handler component:

1. You don't have to manage and maintain a separate component to exclusively handle interruption events.
2. Karpenter's native interruption handling coordinates with other deprovisioning so that consolidation, expiration, etc. can be aware of interruption events and vice-versa.

#### Why am I receiving QueueNotFound errors when I set `--interruption-queue`?

Karpenter requires a queue to exist that receives event messages from EC2 and health services in order to handle interruption messages properly for nodes.

Details on the types of events that Karpenter handles can be found in the \[Interruption Handling Docs]\(\{{< ref "./concepts/disruption/#interruption" >\}}).

Details on provisioning the SQS queue and EventBridge rules can be found in the \[Getting Started Guide]\(\{{< ref "./getting-started/getting-started-with-karpenter/#create-the-karpenter-infrastructure-and-iam-roles" >\}}).

### Consolidation

#### Why do I sometimes see an extra node get launched when updating a deployment that remains empty and is later removed?

Consolidation packs pods tightly onto nodes which can leave little free allocatable CPU/memory on your nodes. If a deployment uses a deployment strategy with a non-zero `maxSurge`, such as the default 25%, those surge pods may not have anywhere to run. In this case, Karpenter will launch a new node so that the surge pods can run and then remove it soon after if it's not needed.

### Logging

#### How do I customize or configure the log output?

Karpenter uses [uber-go/zap](https://github.com/uber-go/zap) for logging. You can customize or configure the log messages by editing the [configmap-logging.yaml](https://github.com/aws/karpenter/blob/main/charts/karpenter/templates/configmap-logging.yaml) `ConfigMap`'s [data.zap-logger-config](https://github.com/aws/karpenter/blob/main/charts/karpenter/templates/configmap-logging.yaml#L26) field. The available configuration options are specified in the [zap.Config godocs](https://pkg.go.dev/go.uber.org/zap#Config).
