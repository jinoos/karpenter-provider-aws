# Documentation

Karpenter는 Kubernetes를 위해 만들어진 오픈소스 노드 수명주기 관리 프로젝트입니다. Kubernetes 클러스터에 Karpenter를 추가하면 해당 클러스터에서 워크로드를 실행하는 효율성과 비용을 크게 개선할 수 있습니다. Karpenter는 다음과 같이 작동합니다:

* **감시** - Kubernetes 스케줄러가 스케줄 불가능으로 표시한 파드를 **감시**
* **평가** - 파드가 요청한 스케줄링 제약 조건(리소스 요청, 노드셀렉터, 어피니티, 톨러레이션, 토폴로지 분배 제약)을 **평가**
* **프로비저닝** - 파드의 요구사항을 충족하는 노드를 **프로비저닝**
* **제거** - 더 이상 필요하지 않은 노드를 **제거**

Karpenter 사용자는 Kubernetes 클러스터와 Karpenter 컨트롤러가 실행되면(\[시작하기]\(\{{\<ref "./getting-started" >\}}) 참조), 다음을 수행할 수 있습니다:

* **NodePool 설정**: Karpenter에 NodePool을 적용하여 노드 프로비저닝에 대한 제약 조건을 구성하고 노드 만료, 노드 통합 또는 Kubelet 구성 값을 설정할 수 있습니다. Kubernetes와 클라우드 공급자(예: AWS)와 관련된 NodePool 수준의 제약 조건은 다음과 같습니다:
  * 테인트(`taints`): 프로비저닝된 노드에 추가할 테인트를 지정합니다. 파드에 테인트와 일치하는 톨러레이션이 없으면 테인트에 의해 설정된 효과(NoSchedule, PreferNoSchedule 또는 NoExecute)가 발생합니다. 자세한 내용은 Kubernetes [테인트와 톨러레이션](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/)을 참조하세요.
  * 레이블(`labels`): 파드가 매칭할 수 있는 임의의 키-값 쌍을 노드에 적용합니다.
  * 요구사항(`requirements`): [잘 알려진 레이블](https://kubernetes.io/docs/reference/labels-annotations-taints/)과 \[클라우드별 설정]\(\{{\<ref "./concepts/nodeclasses" >\}})을 기반으로 노드 프로비저닝에 대한 허용 가능한(`In`) 값과 허용할 수 없는(`Out`) Kubernetes 및 Karpenter 값을 설정합니다. 여기에는 [인스턴스 유형](https://kubernetes.io/docs/reference/labels-annotations-taints/#nodekubernetesioinstance-type), [존](https://kubernetes.io/docs/reference/labels-annotations-taints/#topologykubernetesiozone), [컴퓨터 아키텍처](https://kubernetes.io/docs/reference/labels-annotations-taints/#kubernetes-io-arch), \[용량 유형]\(\{{\<ref "./concepts/nodepools/#capacity-type" >\}}) (예: AWS 스팟 또는 온디맨드)이 포함될 수 있습니다.
  * 제한(`limits`): 클러스터에서 사용할 수 있는 총 CPU와 메모리에 대한 제한을 설정하여 해당 제한에 도달하면 추가 노드 프로비저닝을 중지합니다.
* **워크로드 배포**: 워크로드를 배포할 때 스케줄링 제약 조건을 충족하도록 요청하여 Karpenter가 해당 워크로드에 대해 프로비저닝하는 노드를 지정할 수 있습니다. 파드를 배포할 때 다음과 같은 Pod 스펙 제약 조건을 사용하세요:
  * 리소스(`resources`): 파드에 대한 메모리와 CPU의 요청과 제한을 설정합니다. 자세한 내용은 [요청과 제한](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#requests-and-limits)을 참조하세요.
  * 노드(`nodeSelector`): [nodeSelector](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector)를 사용하여 하나 이상의 선택된 키-값 쌍을 포함하는 노드와 매칭하도록 요청합니다. 이는 사용자가 정의한 임의의 레이블, Kubernetes 잘 알려진 레이블 또는 Karpenter 레이블일 수 있습니다.
  * 노드 어피니티(`NodeAffinity`): [nodeAffinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity)를 설정하여 `nodeSelectorTerms`가 설정되거나 설정되지 않은 노드에서 파드가 실행되도록 합니다. 매칭 어피니티는 특정 운영 체제나 존일 수 있습니다. 노드 어피니티를 필수 또는 선호로 설정할 수 있습니다. `NotIn`과 `DoesNotExist`를 사용하여 노드 안티-어피니티 동작을 정의할 수 있습니다.
  * 파드 어피니티와 안티-어피니티(`podAffinity/podAntiAffinity`): 특정 파드가 실행 중인지(`podAffinity`) 또는 실행되지 않는지(`podAntiAffinity`)에 따라 노드에서 파드를 실행하도록 선택합니다. 자세한 내용은 [파드 간 어피니티와 안티-어피니티](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#inter-pod-affinity-and-anti-affinity)를 참조하세요.
  * 톨러레이션(`tolerations`): 파드가 노드에서 실행되기 전에 노드의 테인트와 일치(톨러레이트)해야 함을 지정합니다. 톨러레이션이 없으면 테인트에 의해 설정된 효과(NoSchedule, PreferNoSchedule 또는 NoExecute)가 발생합니다. 자세한 내용은 Kubernetes [테인트와 톨러레이션](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/)을 참조하세요.
  * 토폴로지 분배(`topologySpreadConstraints`): 파드가 존(`topology.kubernetes.io/zone`), 호스트(`kubernetes.io/hostname`) 또는 클라우드 공급자 용량 유형(`karpenter.sh/capacity-type`) 전체에 분산되도록 요청합니다. 자세한 내용은 [파드 토폴로지 분배 제약 조건](https://kubernetes.io/docs/concepts/workloads/pods/pod-topology-spread-constraints/)을 참조하세요.
  * 영구 볼륨 토폴로지: 파드에 해당 스토리지를 노드에서 사용할 수 있도록 하는 특정 존에서 실행되는 노드가 필요한 스토리지 요구사항이 있음을 나타냅니다.

Karpenter에 대해 자세히 알아보고 시작하는 방법은 아래를 참조하세요.
