---
title: FAQs
linkTitle: FAQs
weight: 60
description: Karpenter 자주 묻는 질문 리뷰
---

# FAQs

### General

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

### Compatibility

#### Karpenter는 어떤 버전의 Kubernetes를 지원하나요?

출시된 Karpenter 버전별 지원되는 Kubernetes 버전을 확인하려면 \[업그레이드 섹션의 호환성 매트릭스]\(\{{< ref "./upgrading/compatibility#compatibility-matrix" >\}})를 참조하세요.

#### 어떤 Kubernetes 배포판이 지원되나요?

Karpenter는 최신 AWS Elastic Kubernetes Service(EKS)의 신규 또는 기존 설치와의 통합을 문서화하고 있습니다. 다른 Kubernetes 배포판(KOPs 등)도 사용할 수 있지만, 이러한 배포판에 대한 클라우드 제공자 권한 설정은 문서화되어 있지 않습니다.

#### Karpenter는 AWS 노드 그룹 기능과 어떻게 상호작용하나요?

NodePool은 EKS 관리형 노드 그룹 및 EC2 Auto Scaling 그룹과 같은 정적 용량 관리 솔루션과 함께 작동하도록 설계되었습니다. NodePool을 사용하여 모든 용량을 관리하거나, 동적 및 정적으로 관리되는 용량을 혼합한 모델을 사용하거나, 완전히 정적인 접근 방식을 사용할 수 있습니다. 대부분의 사용자는 단기적으로는 혼합 접근 방식을, 장기적으로는 NodePool 관리 방식을 사용할 것으로 예상됩니다.

#### Karpenter는 Kubernetes 기능과 어떻게 상호작용하나요?

* Kubernetes Cluster Autoscaler: Karpenter는 Cluster Autoscaler와 함께 작동할 수 있습니다. 자세한 내용은 \[Kubernetes Cluster Autoscaler]\(\{{< ref "./concepts/#kubernetes-cluster-autoscaler" >\}})를 참조하세요.
* Kubernetes Scheduler: Karpenter는 Kubernetes 스케줄러가 스케줄 불가능으로 표시한 파드의 스케줄링에 중점을 둡니다. Karpenter가 Kubernetes 스케줄러와 어떻게 상호작용하는지에 대한 자세한 내용은 \[스케줄링]\(\{{< ref "./concepts/scheduling" >\}})을 참조하세요.

### Provisioning

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

#### Karpenter가 IPv6를 지원하나요?

네! Karpenter는 kube-dns 서비스의 cluster-ip를 확인하여 IPv6 클러스터에서 실행 중인지 동적으로 감지합니다. AL2와 같은 AMI Family를 사용할 때, Karpenter는 자동으로 IPv6용 EKS Bootstrap 스크립트를 구성합니다. 일부 EC2 인스턴스 유형은 IPv6를 지원하지 않으며, Amazon VPC CNI는 Nitro 하이퍼바이저에서 실행되는 인스턴스 유형만 지원합니다. NodePool에 Nitro 인스턴스 유형만 허용하도록 요구사항을 추가하는 것이 가장 좋습니다:Does Karpenter support IPv6?

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

Amazon VPC CNI에서 IPv6를 활성화하는 방법에 대한 자세한 내용은 [문서](https://docs.aws.amazon.com/eks/latest/userguide/cni-ipv6.html)를 참조하세요.

\{\{% alert title="Windows Support Notice" color="warning" %\}}\
Windows 노드는 IPv6를 지원하지 않습니다. \
\{\{% /alert %\}}

#### 대기 중인 파드를 스케줄링하기 위해, 왜 추가 노드가 시작되었다가 비어있는 상태로 나중에 제거되는 것을 보게 되나요?

데몬셋, userData 구성 또는 노드가 프로비저닝된 후 테인트를 적용하는 다른 워크로드가 있을 수 있습니다. 테인트가 적용된 후, Karpenter는 추가된 테인트로 인해 파드가 이 새로운 노드에 스케줄링될 수 없음을 감지합니다. 결과적으로 Karpenter는 또 다른 노드를 프로비저닝합니다. 일반적으로 원래 노드의 테인트가 제거되고 파드가 해당 노드에 스케줄링되면, 추가된 새 노드는 사용되지 않은 채로 남아 비어있음/통합에 의해 제거됩니다. 테인트가 충분히 빨리 제거되지 않으면, Karpenter가 비어있음 통합을 통해 파드가 스케줄링되기 전에 원래 노드를 제거할 수 있습니다. 이로 인해 대기 중인 파드가 스케줄링되지 않은 채로 노드가 계속해서 프로비저닝되고 통합되는 무한 루프가 발생할 수 있습니다.

해결책은 \[startupTaints]\(\{{\<ref "./concepts/nodepools/#cilium-startup-taint" >\}})를 구성하여 Karpenter가 노드가 파드를 받을 준비가 되지 않은 상태에서 파드가 스케줄링되지 않도록 하는 데 필요한 임시 테인트를 인식하도록 하는 것입니다.

다음은 Cilium의 시작 테인트 예시입니다.

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

#### 선호하는 스케줄링 제약 조건을 사용할 때, Karpenter는 처음에 올바른 수의 노드를 시작합니다. 그런데 왜 때때로 즉시 통합되나요?

`kube-scheduler`는 파드의 스케줄링을 담당하고, Karpenter는 용량을 시작합니다. 선호하는 스케줄링 제약 조건을 사용할 때, `kube-scheduler`는 가능한 경우 언제든지 노드에 파드를 스케줄링합니다.

예를 들어, 선호하는 영역 토폴로지 분배로 디플로이먼트를 스케일 업하고 새로 생성된 파드가 기존 클러스터에서 실행될 수 없다고 가정해보겠습니다. Karpenter는 이 선호도를 충족하기 위해 여러 노드를 시작합니다. 만약 a) 한 노드가 다른 노드보다 약간 더 빨리 준비되고 b) 여러 파드를 수용할 수 있는 용량이 있다면, `kube-scheduler`는 가능한 한 많은 파드를 단일 준비된 노드에 스케줄링하여 스케줄링 불가능한 상태가 되지 않도록 합니다. 몇 초 안에 준비될 진행 중인 용량은 고려하지 않습니다. 모든 파드가 단일 노드에 맞는 경우, Karpenter가 시작한 나머지 노드는 준비되었을 때 필요하지 않게 되어 통합 과정에서 삭제됩니다.

#### 클러스터에 추가 DaemonSet을 배포할 때, Karpenter가 추가 DaemonSet을 지원하기 위해 노드를 스케일 업하지 않는 이유는 무엇인가요?

Karpenter는 추가 DaemonSet을 위해 자체적으로 더 많은 용량을 스케일 업하지 않습니다. 이는 새 노드에 스케줄링될 유일한 파드가 DaemonSet 파드이며, 이는 이점 없이 추가 용량을 소비하기 때문입니다. 따라서 Karpenter는 워크로드 파드에 대한 스케일 업을 위한 오버헤드 계산을 할 때만 DaemonSet을 고려합니다.

새로운 DaemonSet이 기존 노드에 스케줄링되지 못하는 것을 방지하기 위해, [DaemonSet 파드에 `preemptionPolicy: PreemptLowerPriority`로 높은 우선순위를 설정](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/#example-priorityclass)하여 DaemonSet 파드가 모든 기존 및 새 노드에 스케줄링되도록 보장해야 합니다. 기존 노드의 경우, 이로 인해 우선순위가 낮은 일부 파드가 [선점](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/#preemption)되어 DaemonSet으로 대체되고, Karpenter가 새로운 대기 중인 파드에 대응하여 시작하는 새로운 용량에 다시 스케줄링됩니다.

Karpenter 유지보수 팀은 또한 파드에 [우선순위나 선점](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/)을 설정하지 않고도 새로운 DaemonSet이 배포될 때 기존 용량을 롤링할 수 있게 하는 통합 메커니즘을 [이 Github 이슈](https://github.com/aws/karpenter/issues/3256)에서 논의하고 있습니다.

#### 내 토폴로지 분배 제약 조건이 영역 전체에 파드를 분배하지 않는 이유는 무엇인가요?

Karpenter는 `topologySpreadConstraints`에 따라 노드를 프로비저닝합니다. 하지만 `minDomains` 필드가 설정되지 않은 경우 Kubernetes 스케줄러는 영역 분배 제약 조건을 충족하지 않는 노드에 파드를 스케줄링할 수 있습니다. Karpenter가 필요한 수보다 더 많은 파드를 처리할 수 있는 노드를 시작하고, 새로 시작된 노드가 서로 다른 시간에 초기화되면, Kubernetes 스케줄러는 준비된 첫 번째 노드에 원하는 수보다 더 많은 파드를 배치할 수 있습니다.

선호되는 해결책은 Kubernetes 1.27부터 기본적으로 활성화되는 [`topologySpreadConstraints`의 `minDomains` 필드](https://kubernetes.io/docs/concepts/scheduling-eviction/topology-spread-constraints/#topologyspreadconstraints-field)를 사용하는 것입니다.

`minDomains`가 사용 가능하기 전에는, 영역 전체에 분배하려는 파드를 시작하기 전에 각 영역에 낮은 [우선순위](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/)의 일시 중지 컨테이너를 시작하는 것이 다른 해결 방법이었습니다. 이러한 일시 중지 파드의 낮은 우선순위는 원하는 파드가 스케줄링될 때 선점된다는 것을 의미합니다.

### Workloads

**파드를 배포하는 사람은 어떻게 Karpenter를 활용할 수 있나요?**

Karpenter가 노드와 파드 요청을 매칭하는 방법에 대한 설명은 \[애플리케이션 개발자]\(\{{< ref "./concepts/#application-developer" >\}})를 참조하세요.

**가용성 영역별로 EBS 디스크와 함께 Karpenter를 사용할 수 있나요?**

네. 자세한 내용은 \[영구 볼륨 토폴로지]\(\{{< ref "./concepts/scheduling#persistent-volume-topology" >\}})를 참조하세요.

**노드에서 `--max-pods`를 설정할 수 있나요?**

네, 자세한 내용은 \[NodePool 문서의 KubeletConfiguration 섹션]\(\{{\<ref "./concepts/nodepools#spectemplatespeckubelet" >\}})을 참조하세요.

**Windows2019와 Windows2022 AMI 제품군이 Windows Server Core만 지원하는 이유는 무엇인가요?**

Core와 Full 변형의 차이점은 Core가 더 적은 구성 요소를 가진 최소한의 OS이며 그래픽 사용자 인터페이스(GUI)나 데스크톱 환경이 없다는 것입니다. `Windows2019`와 `Windows2022` AMI 제품군은 단순성을 위해 Windows Server Core 옵션을 사용하지만, 필요한 경우 Windows Server Full을 실행하도록 사용자 지정 AMI를 지정할 수 있습니다.

Kubernetes 1.31용 Windows Server 2022 Full이 포함된 [Amazon EKS 최적화 AMI](https://docs.aws.amazon.com/eks/latest/userguide/eks-optimized-windows-ami.html)는 AMI 이름을 참조하는 `amiSelector`를 구성하여 지정할 수 있습니다.

```yaml
amiSelectorTerms:
    - name: Windows_Server-2022-English-Full-EKS_Optimized-1.31*
```

**Karpenter를 사용하여 워크로드의 파드를 스케일링할 수 있나요?**

Karpenter는 스케줄링할 수 없는 파드에 대응하여 새로운 노드를 생성하는 노드 자동 스케일러입니다. 파드 자체의 스케일링은 그 범위를 벗어납니다. 이는 [Vertical Pod Autoscaler](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler)(개별 파드의 리소스 스케일링용) 또는 [Horizontal Pod Autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)(복제본 스케일링용)와 같은 파드 자동 스케일러의 영역입니다. 파드에 대한 더 고급 자동 스케일링 기능을 찾고 계시다면 [Keda](https://keda.sh/)도 살펴보시는 것을 추천합니다.

### Deprovisioning

#### Karpenter는 어떻게 노드를 프로비저닝 해제하나요?

Karpenter가 노드를 프로비저닝 해제하는 방법에 대한 정보는 \[노드 프로비저닝 해제]\(\{{< ref "./concepts/disruption" >\}})를 참조하세요.

### Upgrading Karpenter

#### Karpenter를 어떻게 업그레이드하나요?

Karpenter는 클러스터에서 실행되는 컨트롤러이지만, Cluster Autoscaler와 달리 특정 Kubernetes 버전에 종속되지 않습니다. 기존의 업그레이드 메커니즘을 사용하여 Kubernetes의 핵심 애드온을 업그레이드하고 Karpenter를 버그 수정과 새로운 기능으로 최신 상태로 유지하세요.

Karpenter는 `KarpenterNode IAM Role`과 `KarpenterController IAM Role`에 적절한 권한이 필요합니다. Karpenter를 `$VERSION` 버전으로 업그레이드하려면, `KarpenterNode IAM Role`과 `KarpenterController IAM Role`이 `https://karpenter.sh/$VERSION/getting-started/getting-started-with-karpenter/cloudformation.yaml`에 설명된 올바른 권한을 가지고 있는지 확인하세요. 다음으로, `KarpenterController IAM Role` ARN(즉, KarpenterController IAM Role 생성에서 생성된 리소스의 ARN)을 찾아 Helm 업그레이드 명령에 전달하세요. \{\{% script file="./content/en/{VERSION}/getting-started/getting-started-with-karpenter/scripts/step08-apply-helm-chart.sh" language="bash"%\}}

Karpenter 업그레이드에 대한 자세한 정보는 \[업그레이드 가이드]\(\{{< ref "./upgrading/upgrade-guide/" >\}})를 참조하세요.

### Upgrading Kubernetes Cluster

#### How do I upgrade an EKS Cluster with Karpenter?

\{\{% alert title="Note" color="primary" %\}}\
Karpenter는 프로덕션 환경에서 사용하기 전에 항상 하위 환경에서 AMI를 검증할 것을 권장합니다. AMI 업그레이드에 대한 모범 사례를 이해하려면 \[AMI 관리]\(\{{\<ref "./tasks/managing-amis" >\}})를 읽어보세요.

사용자 지정 AMI를 사용하는 경우, \[`amiSelector`]\(\{{\<ref "./concepts/nodeclasses#specamiselectorterms" >\}})와 일치하는 태그가 있는 새 AMI를 게시하거나 \[`amiSelector`]\(\{{\<ref "./concepts/nodeclasses#specamiselectorterms" >\}}) 필드를 변경하여 새 워커 노드 이미지의 롤아웃을 트리거해야 합니다.\
\{\{% /alert %\}}

Karpenter의 기본 동작은 Amazon EKS 클러스터를 업그레이드했을 때 노드를 업그레이드합니다. Karpenter는 EKS 컨트롤 플레인 버전과 동기화를 유지하기 위해 노드를 \[드리프트]\(\{{\<ref "./concepts/disruption#drift" >\}})합니다. 드리프트는 `v0.33`부터 기본적으로 활성화됩니다. 이는 클러스터가 업그레이드되는 즉시 Karpenter가 해당 버전의 새로운 AMI를 자동으로 발견한다는 것을 의미합니다.

[EKS 클러스터 컨트롤 플레인 업그레이드](https://docs.aws.amazon.com/eks/latest/userguide/update-cluster.html)부터 시작하세요. EKS 클러스터 업그레이드가 완료되면, Karpenter는 먼저 대체 노드를 시작하여 이전 클러스터 버전용 EKS 최적화 AMI를 사용하는 Karpenter 프로비저닝된 노드를 드리프트하고 중단시킵니다. Karpenter는 [Pod Disruption Budgets](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/)(PDB)를 준수하고, 자동으로 \[해당 노드들을 교체, 코든, 드레인]\(\{{\<ref "./concepts/disruption#control-flow" >\}})합니다. 파드가 새 노드로 이동하는 것을 최적으로 지원하기 위해, 적절한 파드 [Resource Quotas](https://kubernetes.io/docs/concepts/policy/resource-quotas/)를 설정하고 PDB를 사용하는 Kubernetes 모범 사례를 따르세요.

### Interruption Handling

#### Karpenter 중단 처리와 Node Termination Handler를 함께 사용해야 하나요?

아니요. 두 컴포넌트가 동일한 이벤트를 처리할 때 발생할 수 있는 충돌 때문에 Karpenter와 함께 Node Termination Handler를 사용하지 않는 것을 권장합니다.

#### Node Termination Handler에서 마이그레이션해야 하는 이유는 무엇인가요?

Karpenter의 네이티브 중단 처리는 독립형 Node Termination Handler 컴포넌트에 비해 두 가지 주요 이점을 제공합니다:

1. 중단 이벤트를 독점적으로 처리하기 위한 별도의 컴포넌트를 관리하고 유지할 필요가 없습니다.
2. Karpenter의 네이티브 중단 처리는 다른 프로비저닝 해제와 조정하여 통합, 만료 등이 중단 이벤트를 인식할 수 있고 그 반대도 가능합니다.

#### `--interruption-queue`를 설정할 때 QueueNotFound 오류가 발생하는 이유는 무엇인가요?

Karpenter는 노드에 대한 중단 메시지를 적절히 처리하기 위해 EC2와 헬스 서비스로부터 이벤트 메시지를 수신하는 큐가 존재해야 합니다.

Karpenter가 처리하는 이벤트 유형에 대한 자세한 내용은 \[중단 처리 문서]\(\{{< ref "./concepts/disruption/#interruption" >\}})에서 확인할 수 있습니다.

SQS 큐와 EventBridge 규칙 프로비저닝에 대한 자세한 내용은 \[시작 가이드]\(\{{< ref "./getting-started/getting-started-with-karpenter/#create-the-karpenter-infrastructure-and-iam-roles" >\}})에서 확인할 수 있습니다.

### Consolidation

#### 디플로이먼트를 업데이트할 때 때때로 빈 상태로 유지되다가 나중에 제거되는 추가 노드가 시작되는 것을 보는 이유는 무엇인가요?

통합은 파드를 노드에 빽빽하게 패킹하여 노드에 할당 가능한 CPU/메모리가 거의 남지 않을 수 있습니다. 디플로이먼트가 기본값 25%와 같은 0이 아닌 `maxSurge`를 사용하는 배포 전략을 사용하는 경우, 이러한 서지 파드가 실행될 곳이 없을 수 있습니다. 이 경우 Karpenter는 서지 파드가 실행될 수 있도록 새 노드를 시작한 다음, 필요하지 않은 경우 곧바로 제거합니다.

### Logging

#### 로그 출력을 어떻게 사용자 정의하거나 구성할 수 있나요?

Karpenter는 로깅에 [uber-go/zap](https://github.com/uber-go/zap)을 사용합니다. [configmap-logging.yaml](https://github.com/aws/karpenter/blob/main/charts/karpenter/templates/configmap-logging.yaml)의 `ConfigMap`의 [data.zap-logger-config](https://github.com/aws/karpenter/blob/main/charts/karpenter/templates/configmap-logging.yaml#L26) 필드를 편집하여 로그 메시지를 사용자 정의하거나 구성할 수 있습니다. 사용 가능한 구성 옵션은 [zap.Config godocs](https://pkg.go.dev/go.uber.org/zap#Config)에 명시되어 있습니다.
